---
title: "Stripe Failed Payment Recovery: Step-by-Step Guide for Indie Hackers"
description: "A practical guide to setting up Stripe failed payment recovery for your indie SaaS. Covers webhooks, retry logic, reminder emails, and how to automate the whole thing with DunningLite."
pubDate: "2026-04-10"
tags: ["Stripe failed payment recovery", "indie hacker dunning", "Stripe webhook", "subscription churn", "payment retry"]
---

One of my subscriptions failed last month. I found out when I noticed the revenue was down in my Stripe dashboard. By the time I saw it, the customer had already churned — Stripe had cancelled the subscription after retrying a few times, and the customer had already moved on.

I lost roughly $50 in monthly recurring revenue because I had no recovery process.

This guide is the thing I wish I had read before that happened. It covers how Stripe handles failed payments, where Stripe's built-in tools fall short, and how to build a recovery workflow that actually catches the revenue that would otherwise slip through.

---

## How Stripe Handles Failed Payments (What You Get by Default)

When a subscription payment fails in Stripe, here's what happens by default:

1. Stripe marks the invoice as `past_due`
2. Stripe fires a `invoice.payment_failed` webhook event
3. Stripe's Smart Retries automatically retries the charge at intervals determined by its ML model (typically 3–4 retries over 7–14 days)
4. If all retries fail, Stripe marks the subscription as `canceled` or `unpaid` depending on your settings

This is... fine. The retry logic works. The problem is everything Stripe doesn't do:

- Stripe doesn't email your customers about the failure
- Stripe doesn't tell customers when a retry is coming
- Stripe doesn't give you a dashboard showing your recovery status
- Stripe doesn't let you customize the retry schedule
- Stripe doesn't let you pause cancellation while the customer updates their card

The result: payments fail silently, customers have no idea there's a problem, and by the time anyone notices, the subscription has already been cancelled.

---

## The Stripe Webhook Events You Need to Know

Before building any recovery workflow, you need to understand which events to listen for.

**`invoice.payment_failed`**
Fires every time a payment attempt fails. This is your primary trigger. The event payload includes the customer ID, invoice ID, subscription ID, and failure reason.

**`invoice.payment_action_required`**
Fires when a payment requires 3D Secure authentication. The customer needs to complete an authentication step — retrying the card won't work until they do.

**`customer.subscription.updated`**
Fires when subscription status changes. Watch for transitions to `past_due`, `unpaid`, or `canceled` status.

**`customer.subscription.deleted`**
Fires when a subscription is finally cancelled. This is your signal that the recovery window has closed.

Here's a basic webhook handler in Node.js to capture these events:

```javascript
app.post('/api/webhooks/stripe', express.raw({ type: 'application/json' }), async (req, res) => {
  const sig = req.headers['stripe-signature'];
  let event;

  try {
    event = stripe.webhooks.constructEvent(req.body, sig, process.env.STRIPE_WEBHOOK_SECRET);
  } catch (err) {
    return res.status(400).send(`Webhook Error: ${err.message}`);
  }

  switch (event.type) {
    case 'invoice.payment_failed':
      await handlePaymentFailed(event.data.object);
      break;
    case 'customer.subscription.updated':
      await handleSubscriptionUpdated(event.data.object);
      break;
    case 'customer.subscription.deleted':
      await handleSubscriptionCancelled(event.data.object);
      break;
  }

  res.json({ received: true });
});
```

---

## Building the Retry Schedule

Stripe's Smart Retries handles the actual charge retries. But if you want control over timing — or want to add email touchpoints before each retry — you need to manage your own retry queue.

Here's the retry logic DunningLite uses:

```javascript
async function scheduleRetries(failedPayment) {
  const retryIntervals = [
    { days: 1, label: 'retry_1' },
    { days: 3, label: 'retry_2' },
    { days: 7, label: 'retry_3' },
  ];

  for (const interval of retryIntervals) {
    const scheduledAt = new Date();
    scheduledAt.setDate(scheduledAt.getDate() + interval.days);

    await db.insert('payment_retries', {
      payment_id: failedPayment.id,
      customer_id: failedPayment.customer,
      invoice_id: failedPayment.invoice,
      scheduled_at: scheduledAt,
      status: 'pending',
      attempt_label: interval.label,
    });
  }
}
```

A cron job (Vercel Cron in DunningLite's case) runs hourly and picks up pending retries that are due:

```javascript
async function processRetries() {
  const dueRetries = await db.query(
    'SELECT * FROM payment_retries WHERE scheduled_at <= NOW() AND status = "pending"'
  );

  for (const retry of dueRetries) {
    try {
      await stripe.invoices.pay(retry.invoice_id);
      await db.update('payment_retries', retry.id, { status: 'recovered' });
      await notifyOwner(retry, 'recovered');
    } catch (err) {
      await db.update('payment_retries', retry.id, {
        status: 'failed',
        failure_reason: err.message
      });
    }
  }
}
```

The key thing here: you're calling `stripe.invoices.pay()` manually. This bypasses Stripe's Smart Retries and gives you full control over timing.

---

## Writing Reminder Emails That Actually Work

The technical part is straightforward. The part that actually determines your recovery rate is the emails.

A few principles:

**Send the email before the retry, not after.** If you email the customer 24 hours before each retry, they have time to update their card. The charge goes through on the scheduled date, recovery happens automatically.

**Don't make it scary.** "URGENT: PAYMENT FAILED — YOUR ACCOUNT WILL BE SUSPENDED" triggers immediate panic and sometimes a cancellation request, even from customers who wanted to fix it. Try this instead:

> Subject: Heads up — there was an issue with your payment for [Product Name]
>
> Hey [Name],
>
> Just a quick note — we weren't able to process your last payment. No action needed right now, we'll automatically try again on [date].
>
> If you want to update your payment method before then, you can do that here: [link]
>
> Questions? Just reply to this email.
>
> [Your name]

Notice what that email does:
- Leads with "heads up" not "urgent"
- Makes it clear they don't have to do anything immediately
- Tells them exactly when the retry is happening
- Gives them the option to fix it proactively
- Has a human sign-off, not "The Team"

**Personalize where you can.** Even just using their first name and the product name makes it feel less automated. Customers who get a generic payment-failure email from "noreply@stripe.com" often assume it's spam.

**The third email should be more urgent.** The +7 day email is your last chance before cancellation. This one can and should be more direct: "This is our final attempt" + clear consequences + easy fix link.

---

## Step-by-Step: Setting Up Stripe Failed Payment Recovery

Here's the complete setup process using DunningLite:

### Step 1: Deploy DunningLite

```bash
git clone https://fixitdev.com/tools/dunninglite
cd dunninglite
npm install
```

Copy `.env.local.example` to `.env.local` and fill in:

```
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key
STRIPE_SECRET_KEY=sk_live_...
RESEND_API_KEY=re_...
NEXT_PUBLIC_APP_URL=https://your-domain.com
CRON_SECRET=any-random-string
```

### Step 2: Run the Database Migration

In your Supabase project, open the SQL editor and run the contents of `supabase/migrations/001_initial_schema.sql`. This creates the tables for tracking failed payments and retries.

### Step 3: Deploy to Vercel

```bash
vercel deploy
```

Make sure to add your environment variables in the Vercel project settings. The `vercel.json` includes the cron configuration that runs the retry job every hour automatically.

### Step 4: Configure Stripe Webhook

1. Open your Stripe Dashboard
2. Go to Developers > Webhooks > Add endpoint
3. Set the URL to `https://your-dunninglite-domain.com/api/webhooks/stripe`
4. Select these events: `invoice.payment_failed`, `customer.subscription.updated`
5. Click Add endpoint
6. Copy the signing secret

### Step 5: Add Webhook Secret to DunningLite

Go to your deployed DunningLite instance, open Settings, and paste the Stripe webhook signing secret. This verifies that webhook events are genuinely from Stripe.

### Step 6: Configure Email Templates

In the Email settings, customize the three email templates with your product name, brand voice, and support contact. Pro users can fully customize the HTML templates.

That's it. From this point on, every failed payment in your Stripe account will automatically enter the recovery queue, get reminder emails, and be retried on schedule.

---

## Monitoring Your Recovery Performance

Once you're live, check your DunningLite dashboard weekly:

**Recovery rate by failure reason**: Are you recovering more soft declines than hard declines? That's expected — hard declines (expired cards, account closed) require customer action. If your recovery rate is low across the board, your emails might not be reaching customers or the CTA isn't clear enough.

**Recovery rate by plan**: Higher-paying customers often have more "important" cards on file (company cards, less likely to be declined for insufficient funds). If your lower-tier customers have a much worse recovery rate, consider whether you need a different email strategy for them.

**Time to recovery**: If most recoveries happen on the +1 day retry, great — your emails are helping customers act fast. If most happen on +7, it might mean the early emails aren't getting through.

---

## Common Mistakes in Stripe Failed Payment Recovery

**Not disabling Stripe Smart Retries when you add your own retry logic.** If you're running your own retry schedule AND Stripe Smart Retries is still on, you may be retrying payments multiple times in unexpected patterns. Decide on one or the other.

**Retrying too aggressively.** Multiple retries per day can be flagged as suspicious by some banks. Stick to daily or every-few-days intervals.

**Generic emails from a no-reply address.** Customers mark these as spam. Use a real human-looking sender address (hi@yourproduct.com) and make sure replies actually go somewhere.

**Cancelling too quickly.** If your Stripe settings cancel subscriptions immediately after the first failed payment, your retry system never gets a chance. Set your Stripe subscription settings to `leave_subscription_on_dunning_failure = true` and manage cancellation yourself after the dunning period.

---

## The Bottom Line

Stripe failed payment recovery isn't complicated in principle. Detect the failure, retry a few times with good timing, send friendly emails, track what happens.

The gap between "I'll set this up someday" and actually having it running is mostly just friction. You need webhook handling, a retry queue, email infrastructure, and a way to track recovery status.

[DunningLite](/tools/dunninglite) packages all of that into a deploy-once setup that takes about 5 minutes. Free tier covers the basics. $29 Pro tier unlocks branded emails and custom retry timing.

Whatever you use — please set something up. The failed payments are happening whether you're watching or not.
