---
type: memory
version: 0.1.0
scope: project
domain: engineering
owner: example
created: 2026-01-10
last_updated: 2026-02-28
entry_count: 3
---

# Project Memory — Web App

---

## [MEM-003] Migrated from REST to GraphQL mid-project

**Date:** 2026-02-20  
**Context:** API was getting complex with 15+ endpoints, frontend making too many round trips  
**Tags:** #api #graphql #rest #architecture  
**Confidence:** medium  
**Status:** active  

### Situation
Frontend dashboard was making 6-8 API calls per page load to assemble data from different REST endpoints. Page load times were creeping past 3 seconds. Team suggested migrating to GraphQL to let the frontend request exactly what it needs in one call.

### Decision
- **Chosen:** Migrate to GraphQL using Apollo Server
- **Considered:** Keep REST and add a BFF (backend-for-frontend) layer, batch endpoints
- **Ruled out:** Batch endpoints would just be band-aids

### Reasoning
GraphQL solves the overfetching and underfetching problem cleanly. Apollo has good tooling. The migration could happen incrementally — new endpoints in GraphQL, old ones stay until replaced.

### Outcome
- **Result:** Mixed
- **What happened:** Page load dropped to 1.2 seconds which was great. But the migration took 3 weeks instead of the estimated 1 week. Caching was way harder with GraphQL than REST. N+1 query problems showed up immediately and needed DataLoader to fix.
- **Timeline:** 3 weeks to migrate, another week to fix performance issues

### Lesson
- GraphQL is worth it for complex frontends with varied data needs. But the migration cost is always 2-3x what you estimate.
- Always set up DataLoader from day one with GraphQL. N+1 queries will hit you immediately.
- REST caching is trivial compared to GraphQL caching. Budget time for this.
- Incremental migration (both running side by side) was the right call. Don't do a big bang rewrite.

---

## [MEM-002] Skipped writing tests to ship faster — paid for it

**Date:** 2026-02-05  
**Context:** Tight deadline for a demo, decided to skip tests on the auth module  
**Tags:** #testing #technical-debt #deadline #auth  
**Confidence:** high  
**Status:** active  

### Situation
Had a demo in 3 days. Auth module needed 2 more features (password reset and session management). Writing tests would take an extra day. Decided to ship without tests and add them later.

### Decision
- **Chosen:** Ship without tests, add them after the demo
- **Considered:** Write tests first and risk missing the demo, reduce feature scope

### Reasoning
Demo was with a potential investor. Missing it wasn't an option. Tests could come after. The code was "simple enough" to verify manually.

### Outcome
- **Result:** Negative
- **What happened:** Demo went fine. But the next week, a refactor broke session management silently. Users were getting logged out randomly. Took 2 days to find the bug because there were no tests to catch the regression. The "add tests later" task never got prioritized until the bug forced it.
- **Timeline:** Bug appeared 8 days after shipping, took 2 days to fix

### Lesson
- "Add tests later" means "never add tests until something breaks."
- Auth modules specifically should never ship without tests. Security-critical code is the worst place to cut corners.
- If the deadline is tight, reduce scope instead of removing tests. A smaller feature that works beats a bigger feature that breaks.
- The 1 day "saved" by skipping tests cost 2 days of debugging plus user trust. Net loss every time.

---

## [MEM-001] Chose Next.js over plain React for a simple dashboard

**Date:** 2026-01-10  
**Context:** Starting a new internal analytics dashboard  
**Tags:** #frontend #nextjs #react #architecture #getting-started  
**Confidence:** medium  
**Status:** active  

### Situation
Building an internal dashboard for the team to monitor key metrics. Expected ~5 pages, basic auth, data tables and charts. Team had React experience but no Next.js experience.

### Decision
- **Chosen:** Next.js with App Router
- **Considered:** Plain React with Vite, Remix
- **Ruled out:** Remix (team had zero experience, too much new at once)

### Reasoning
Next.js gives us SSR for faster initial loads, file-based routing which is simpler than React Router for a small app, and API routes so we don't need a separate backend. Seemed like the right tool.

### Outcome
- **Result:** Mixed
- **What happened:** File-based routing was genuinely nice. But the App Router added complexity we didn't need — server components vs client components confusion ate up 2 days. For an internal dashboard with 5 pages and no SEO needs, SSR was overkill. Plain React with Vite would have been simpler and faster to build.
- **Timeline:** Added ~3 days of learning overhead for marginal benefit

### Lesson
- For internal tools with no SEO needs and a small number of pages, plain React with Vite is the faster choice.
- Next.js App Router has a learning curve that isn't justified for simple projects. The mental model of server vs client components creates friction.
- Next.js shines for public-facing apps where SEO, performance, and routing complexity matter.
- Match the tool to the actual need, not the resume line. The simplest tool that solves the problem is the right one.
