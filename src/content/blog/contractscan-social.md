# ContractScan - Social Media Drafts

## Twitter/X

### Post 1 - Launch announcement
I got tired of signing contracts I didn't fully understand. So I built ContractScan -- upload a PDF, get AI risk analysis with a score and suggested edits. Free, no sign-up. https://mvtrdmqv88-droid.github.io/fixitdev-site/tools/contractscan/ #freelance #buildinpublic

### Post 2 - Feature highlight
Your freelance contract has a non-compete clause buried on page 7. A broad IP assignment on page 4. And a net-90 payment term hidden in the definitions. ContractScan catches all of it in 30 seconds. https://mvtrdmqv88-droid.github.io/fixitdev-site/tools/contractscan/

### Post 3 - Engagement question
Freelancers: what's the worst clause you've ever found in a contract AFTER signing it? I'll go first -- unlimited liability with no cap on a $2K project.

---

## Reddit r/freelance

**Title:** I built a free AI tool that reviews freelance contracts and flags red flags

**Body:**

Hey everyone. I've been freelancing for a few years and one thing that always stressed me out was contract review. I couldn't afford a lawyer for every new client, but I also knew I was probably missing important stuff in the fine print.

So I built ContractScan. You upload a contract PDF, and it gives you:

- A risk score (1-100)
- Red flags highlighted with plain-English explanations (liability, IP, non-compete, payment terms, etc.)
- Suggested alternative language you can send back to the client

It's free for 3 scans per day, no account needed. There's a Pro plan ($9.99/mo or $29 one-time) if you need unlimited scans and PDF report downloads.

It's NOT a replacement for a lawyer on big deals. But for standard freelance agreements, it catches the stuff that matters and saves you from signing something you'll regret.

Would love feedback from other freelancers who've dealt with bad contracts. What clauses do you always watch for?

Link: https://mvtrdmqv88-droid.github.io/fixitdev-site/tools/contractscan/

---

## Reddit r/webdev (tech angle)

**Title:** Show r/webdev: ContractScan -- AI contract review built with Next.js 14 + Claude API

**Body:**

Built a tool that solves a real problem I had as a freelance developer: understanding contracts without paying $300+ for a lawyer.

**How it works:** Upload a PDF contract, the app extracts text with pdf-parse, sends it to Claude for structured analysis, and returns a report with risk scoring, red flag detection across 6 categories, and suggested contract edits.

**Tech stack:**
- Next.js 14 (App Router)
- Claude API for analysis
- pdf-parse for PDF text extraction
- Vercel for hosting

**Interesting challenges:**
- Getting consistent structured output from the LLM for 6 different clause categories
- Handling multi-page PDFs that sometimes have weird formatting from scanned documents
- Balancing between giving useful analysis and not pretending to be legal advice

Free tier: 3 scans/day, no sign-up. Pro: $9.99/mo or $29 lifetime.

Happy to answer questions about the tech or the product decisions.

Link: https://mvtrdmqv88-droid.github.io/fixitdev-site/tools/contractscan/

---

## Product Hunt

**Tagline:** AI-powered contract review for freelancers -- upload a PDF, get instant risk analysis

**Description:**

ContractScan helps freelancers understand contracts before they sign. Upload any contract PDF and get:

- Risk score (1-100) so you instantly know if a contract is safe or sketchy
- Red flag detection across 6 key areas: liability, IP assignment, non-compete, payment terms, termination, and indemnification
- Plain-English explanations of every flagged clause
- Copy-paste suggested edits to send back to your client

No legal degree required. Free for 3 scans per day. Pro plan ($9.99/mo or $29 one-time) unlocks unlimited scans and downloadable PDF reports.

Built by FixItDev -- an indie developer who got tired of signing contracts without fully understanding them.

**Maker Comment:**

Hey PH! I'm a freelance developer and I built ContractScan because I kept running into the same problem: a client sends a contract, wants it signed today, and I can't afford to call a lawyer every time.

The last straw was when a client tried to claim ownership of my pre-existing code library, citing a broad IP assignment clause I'd signed without thinking. That one clause could have cost me years of work.

So I built a tool that catches stuff like that automatically. It's not a replacement for a lawyer on big deals, but for your typical freelance agreement, it gives you the confidence to know what you're signing.

The free tier is genuinely useful (3 scans/day, no account needed). If you review contracts regularly, the $29 lifetime option is there so you never worry about it again.

I'd love your feedback -- especially if you're a freelancer who's been burned by a bad contract clause before.

---

## Hacker News

**Title:** Show HN: ContractScan -- AI contract review for freelancers (Next.js + Claude API)

**Text:**

I built a tool that reviews freelance contracts using AI. Upload a PDF, get a risk score, red flags, and suggested edits in about 30 seconds.

The problem: most freelancers can't afford a lawyer for every contract, but signing without understanding the terms is risky. I've personally been burned by a broad IP assignment clause I missed.

Tech: Next.js 14, Claude API for analysis, pdf-parse for PDF extraction. The interesting part was getting structured, consistent output across six clause categories (liability, IP, non-compete, payment, termination, indemnification) while keeping the analysis useful rather than generic.

Free tier: 3 scans/day, no sign-up required. Pro: $9.99/mo or $29 one-time.

https://mvtrdmqv88-droid.github.io/fixitdev-site/tools/contractscan/
