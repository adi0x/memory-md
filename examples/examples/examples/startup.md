---
type: memory
version: 0.1.0
scope: personal
domain: product
owner: example
created: 2026-01-05
last_updated: 2026-02-25
entry_count: 3
---

# Personal Memory — Product & Strategy

---

## [MEM-003] Launched on Twitter before Product Hunt — way better results

**Date:** 2026-02-25  
**Context:** Deciding launch strategy for an open source developer tool  
**Tags:** #launch #twitter #product-hunt #distribution  
**Confidence:** high  
**Status:** active  

### Situation
Had a developer tool ready to launch. Traditional advice says launch on Product Hunt for visibility. But our target audience (AI/crypto developers) lives on Twitter, not Product Hunt.

### Decision
- **Chosen:** Launch on Twitter first with a thread, Product Hunt a week later
- **Considered:** Product Hunt first, simultaneous launch on both
- **Ruled out:** Simultaneous because we couldn't give both full attention

### Reasoning
Your launch platform should match where your users actually are. Our users scroll Twitter, not Product Hunt. A good tweet thread with a demo gets reshared within the community. Product Hunt is good for general visibility but not for niche developer tools.

### Outcome
- **Result:** Positive
- **What happened:** Twitter thread got 340 retweets and 89 stars on GitHub in 48 hours. Product Hunt launch a week later got 67 upvotes but only 12 additional stars. 80% of our early users came from Twitter.
- **Timeline:** First 48 hours determined the trajectory

### Lesson
- Launch where your users are, not where "everyone" launches.
- For developer tools targeting crypto/AI builders, Twitter beats Product Hunt every time.
- The tweet thread format works because people can share individual insights, not just the product link.
- Product Hunt is still worth doing for SEO and credibility, just don't make it your primary channel.

---

## [MEM-002] Built features users didn't ask for instead of fixing what they complained about

**Date:** 2026-02-10  
**Context:** Early stage product with ~50 active users giving feedback  
**Tags:** #product #user-feedback #prioritization #mistakes  
**Confidence:** high  
**Status:** active  

### Situation
Had a working MVP with 50 active users. Users kept complaining about slow load times and confusing navigation. Instead of fixing those, spent 3 weeks building an advanced analytics feature because it seemed more impressive and differentiated.

### Decision
- **Chosen:** Build new analytics feature
- **Considered:** Fix performance and UX issues first, do both in parallel
- **Ruled out:** Fixing existing issues because it felt like "maintenance" not "progress"

### Reasoning
Thought the analytics feature would attract new users and generate buzz. Fixing load times felt unglamorous. Wanted to show investors we were building new things, not just patching.

### Outcome
- **Result:** Negative
- **What happened:** Lost 15 of 50 users during those 3 weeks because the core experience was frustrating. The new analytics feature impressed nobody because users never got to it — they bounced before discovering it. Had to spend the next 2 weeks doing the fixes we should have done first.
- **Timeline:** 3 weeks building the wrong thing, 2 weeks fixing what we should have fixed first, net 5 weeks wasted

### Lesson
- Fix what users complain about before building what you think they want.
- Retention beats acquisition. Keeping 50 users happy creates more growth than impressing 5 new ones.
- "Maintenance" work like performance and UX is not less valuable than new features. It's more valuable because it compounds.
- When 3+ users report the same pain point, drop everything and fix it.

---

## [MEM-001] Tried to build everything alone instead of using existing tools

**Date:** 2026-01-05  
**Context:** Starting a new project, making build-vs-buy decisions  
**Tags:** #build-vs-buy #solo-founder #productivity #tools  
**Confidence:** high  
**Status:** active  

### Situation
Starting a new project as a solo builder. Needed auth, database, hosting, email, and analytics. Decided to build custom auth and deploy on raw AWS instead of using managed services.

### Decision
- **Chosen:** Custom auth system, raw AWS deployment
- **Considered:** Clerk/Auth0 for auth, Vercel/Railway for hosting
- **Ruled out:** Managed services because they "cost too much" and I wanted "full control"

### Reasoning
Wanted to learn the infrastructure deeply. Also thought managed services were too expensive for an early-stage project. Full control seemed important.

### Outcome
- **Result:** Negative
- **What happened:** Spent 2 weeks on auth alone (password hashing, session management, email verification, password reset). AWS deployment took another week of debugging nginx configs and SSL certificates. Managed services would have cost ~$20/month and saved 3 weeks. At solo founder hourly rates, those 3 weeks cost way more than $20/month.
- **Timeline:** 3 weeks lost on infrastructure that adds zero user value

### Lesson
- Solo builders should use managed services for everything that isn't their core product. Auth, hosting, email, analytics — buy all of it.
- "Full control" is a trap when you're building alone. Control over auth doesn't matter if you never ship the actual product.
- Calculate the real cost: your time building infrastructure vs the monthly cost of a managed service. It's never close.
- The only exception: if the infrastructure IS your product, build it. Otherwise, buy.
- This applies to every new project. Always ask: "Is this my product or my plumbing?"
