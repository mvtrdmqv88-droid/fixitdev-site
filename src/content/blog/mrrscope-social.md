# MRRScope - Social Media Drafts

---

## Twitter/X

### Tweet 1 - Launch announcement

Tired of guessing your MRR? I built MRRScope -- connects to Stripe, shows MRR, ARR, Churn, LTV, ARPU instantly. $29 one-time or try the demo free. https://mvtrdmqv88-droid.github.io/fixitdev-site/tools/mrrscope/ #indiehackers #saas #buildinpublic

### Tweet 2 - Feature highlight

Your Stripe dashboard doesn't tell you:
- Your actual MRR
- Your churn rate
- Your customer lifetime value
- Where your revenue growth is coming from

MRRScope does. One Stripe connect, all the numbers.

https://mvtrdmqv88-droid.github.io/fixitdev-site/tools/mrrscope/

#saas #stripe #indiehackers

### Tweet 3 - Engagement question

Honest question for SaaS founders: do you actually know your churn rate?

Not "I think it's around 3-5%." The actual number.

I didn't either, which is why I built MRRScope. It connects to Stripe and calculates it for you. Free demo if you want to see what it looks like.

#saas #buildinpublic

---

## Reddit r/SaaS

**Title:** I built a $29 SaaS metrics dashboard because Baremetrics is too expensive when you're making $500/mo

**Body:**

Hey everyone,

I'm an indie founder and I kept running into the same problem: I wanted to know my MRR, churn rate, and LTV, but every analytics tool out there costs $50-200+/month. When your MRR is $500, that's a painful percentage of your revenue going to a dashboard.

So I built MRRScope. It connects to your Stripe account (read-only) and calculates:

- MRR and ARR
- Customer and revenue churn rate
- LTV (lifetime value)
- ARPU (average revenue per user)
- Revenue breakdown (new, expansion, contraction, churned)

It's $29 one-time or $9.99/mo. There's a full demo mode with sample data if you want to try it without connecting anything.

I'm not trying to compete with Baremetrics or ChartMogul — those are great for bigger companies. This is specifically for indie founders and early-stage SaaS where you just want the core metrics without the enterprise price tag.

Would love feedback. What metrics do you track that I'm missing?

Link: https://mvtrdmqv88-droid.github.io/fixitdev-site/tools/mrrscope/

---

## Reddit r/SideProject

**Title:** Built a Stripe analytics dashboard for indie founders - free demo, $29 for real data

**Body:**

Hey r/SideProject!

Sharing something I just shipped: MRRScope — a SaaS metrics dashboard that connects to Stripe.

**The problem:** Stripe doesn't natively show you MRR, churn rate, or customer lifetime value. The tools that do (Baremetrics, ChartMogul) cost $100+/month. I just wanted to know my numbers without bleeding money.

**The solution:** MRRScope connects to Stripe with one OAuth click and shows you 6 core metrics: MRR, ARR, Churn Rate, LTV, ARPU, and revenue breakdown charts.

**Tech stack:** Next.js 14 + Stripe SDK + Recharts

**Pricing:** Free demo mode (no sign-up). Pro is $29 one-time or $9.99/mo.

**What I learned building this:**
- Stripe's API is actually pretty great for pulling subscription data
- Recharts was the right call for charts — lightweight and customizable
- Demo mode was the best decision I made. People can try everything before paying
- One-time pricing converted way better than I expected

Happy to answer questions about the build or the product. Feedback welcome!

https://mvtrdmqv88-droid.github.io/fixitdev-site/tools/mrrscope/

---

## Reddit r/webdev (tech angle)

**Title:** Building a SaaS metrics calculator with Stripe's API and Next.js 14

**Body:**

Just shipped a project and thought I'd share some technical notes in case anyone is working with Stripe's subscription APIs.

MRRScope is a dashboard that connects to Stripe and calculates SaaS metrics (MRR, ARR, churn, LTV, etc.). Built with Next.js 14 App Router, Stripe SDK, and Recharts.

**Some things I learned:**

1. **Stripe doesn't have an MRR endpoint.** You have to aggregate subscription data yourself. The Subscriptions API gives you individual sub details, but calculating net MRR requires pulling all active subs, normalizing intervals (monthly vs. annual), and tracking changes over time.

2. **Churn calculation is trickier than you'd think.** You need to compare subscription states across time periods. I ended up using Stripe Events to track cancellations and downgrades rather than diffing snapshots.

3. **Recharts is solid for this use case.** Stacked area charts for revenue breakdown, line charts for metric trends. The composability of Recharts components made it easy to build the specific visualizations I needed.

4. **Demo mode with realistic data is worth the effort.** I generate sample subscription data that mimics real patterns (including seasonality and churn spikes). It took extra time to build but dramatically improved the landing page conversion because people can actually use the product before paying.

If anyone's curious about the Stripe subscription aggregation approach, happy to go deeper on that.

https://mvtrdmqv88-droid.github.io/fixitdev-site/tools/mrrscope/

---

## Product Hunt

**Tagline:** Know your MRR without the $100/mo dashboard.

**Description:**

MRRScope connects to your Stripe account and instantly shows you the SaaS metrics that matter: MRR, ARR, Churn Rate, LTV, ARPU, and revenue breakdowns with visual charts.

Built for indie founders and early-stage SaaS who need core metrics without enterprise pricing. Try the full demo with sample data (no sign-up required), then connect Stripe when you're ready.

$29 one-time or $9.99/mo.

**Maker Comment:**

Hey Product Hunt! I'm the indie dev behind MRRScope.

I built this because I was frustrated. Every month I'd open Stripe, see a revenue number, and think "but what's my actual MRR? What's my churn?" Stripe doesn't tell you that. And the tools that do start at $100/mo.

MRRScope is intentionally simple. Six metrics, clean charts, one Stripe connect. It takes about 2 minutes from landing page to seeing your data.

The demo mode uses realistic sample data so you can explore everything before committing. I think that's important — you should know exactly what you're getting before you pay.

Would love your feedback. What's working, what's missing, what would make this more useful for you?

---

## Hacker News

**Title:** Show HN: MRRScope -- SaaS metrics dashboard for indie founders ($29 one-time)

**Text:**

I built MRRScope because Stripe doesn't show you MRR, churn, or LTV natively, and the existing analytics tools (Baremetrics, ChartMogul) are priced for funded startups.

MRRScope connects to Stripe via OAuth (read-only), aggregates your subscription data, and calculates: MRR, ARR, churn rate, LTV, ARPU, and revenue breakdown (new/expansion/contraction/churned).

Built with Next.js 14 and Recharts. There's a demo mode with sample data you can try without creating an account.

Pricing: free demo, $29 one-time or $9.99/mo for real Stripe data.

https://mvtrdmqv88-droid.github.io/fixitdev-site/tools/mrrscope/
