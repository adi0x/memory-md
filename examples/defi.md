---
type: memory
version: 0.1.0
scope: project
domain: defi
owner: adithi
created: 2026-01-15
last_updated: 2026-03-01
entry_count: 5
---

# Project Memory

_This file captures decisions, outcomes, and lessons learned.
Agents read this file to learn from past experience._

---

## [MEM-005] Switched RWA dashboard data source from CoinGecko to DefiLlama

**Date:** 2026-02-20  
**Context:** RWA analytics dashboard was hitting CoinGecko rate limits during peak hours  
**Tags:** #api #rate-limits #data-source #rwa-dashboard  
**Confidence:** high  
**Status:** active  

### Situation
The RWA dashboard was pulling price data from CoinGecko's free API. During US market hours (2-4pm EST), the dashboard would fail for users because we'd hit the rate limit. This was happening 3-4 times per week and users were complaining.

### Decision
- **Chosen:** Migrate primary data source to DefiLlama with CoinGecko as fallback
- **Considered:** Upgrading to CoinGecko paid tier ($129/mo), caching layer with Redis
- **Ruled out:** CoinGecko paid tier because DefiLlama is free and has better DeFi-specific data

### Reasoning
DefiLlama has higher rate limits for free tier, better protocol-level data for DeFi, and the API structure maps more naturally to what we needed. Adding it as primary with CoinGecko as fallback gives us redundancy without cost.

### Outcome
- **Result:** Positive
- **What happened:** Zero rate limit issues in 3 weeks since migration. Dashboard response time actually improved by ~200ms because DefiLlama's endpoints return less unnecessary data.
- **Timeline:** Immediate improvement

### Lesson
- DefiLlama should be the default choice for DeFi protocol data. CoinGecko is better for general crypto price data.
- Always implement a fallback data source for any external API dependency.
- Free tier limitations will always hit at peak hours. Design for this from day one.

---

## [MEM-004] Chose Foundry over Hardhat for ASCEND Protocol testing

**Date:** 2026-02-10  
**Context:** Setting up the testing framework for ASCEND Protocol smart contracts  
**Tags:** #smart-contracts #testing #foundry #hardhat  
**Confidence:** high  
**Status:** active  

### Situation
Needed a testing framework for ASCEND Protocol's on-chain credit scoring contracts. Had previous experience with Hardhat on earlier projects. Team suggested evaluating Foundry.

### Decision
- **Chosen:** Foundry (forge)
- **Considered:** Hardhat, Brownie
- **Ruled out:** Brownie (Python-based, less community support now), Hardhat (slower test execution)

### Reasoning
Foundry tests run in Solidity directly, so no context switching between JS and Solidity. Fuzz testing is built in. Test execution is 10-50x faster than Hardhat for our contract size.

### Outcome
- **Result:** Positive
- **What happened:** Test suite runs in 3 seconds vs estimated 45+ seconds in Hardhat. Fuzz testing caught an edge case in the credit score calculation that manual tests missed. Learning curve was about 2 days.
- **Timeline:** Productivity gain visible within first week

### Lesson
- Foundry is the better choice for Solidity-heavy projects where tests are primarily unit/fuzz tests.
- Hardhat is still better when you need complex deployment scripts with JS/TS or heavy frontend integration testing.
- Fuzz testing should be default, not optional. It caught a bug we wouldn't have found manually.
- Related: [[MEM-002]]

---

## [MEM-003] Provided narrow range liquidity on Uniswap V3 — got hit by IL

**Date:** 2026-01-28  
**Context:** Experimenting with concentrated liquidity strategies for portfolio project  
**Tags:** #liquidity #uniswap #impermanent-loss #risk  
**Confidence:** high  
**Status:** active  

### Situation
Was testing concentrated liquidity positions on Uniswap V3 for the Hyperliquid whale tracker project. Wanted to understand LP dynamics firsthand to build better analytics. Provided ETH/USDC liquidity in a ±5% range around current price.

### Decision
- **Chosen:** Narrow range (±5%) on ETH/USDC
- **Considered:** Wide range (±25%), full range, Curve stable pool
- **Ruled out:** Full range felt too capital-inefficient for the experiment

### Reasoning
Narrow range would capture more fees per dollar of liquidity. Wanted to test the concentrated liquidity thesis with real money (small amount) to understand the mechanics deeply.

### Outcome
- **Result:** Negative
- **What happened:** ETH dropped 8% in 36 hours. Position went out of range. By the time price returned, IL had eaten all accumulated fees plus 12% of principal. Total loss: ~$340 on a $2800 position.
- **Timeline:** 36 hours to go out of range, 5 days for full impact assessment

### Lesson
- Narrow ranges on volatile pairs are a trap unless you're actively managing the position hourly.
- The fee APR looks amazing until a single move wipes out weeks of fees.
- For learning purposes, always use the minimum viable amount. The lesson cost $340 but would have been the same at $50.
- Before providing liquidity, check historical volatility for the pair over the last 30 days. If daily moves regularly exceed your range, you will get rekt.

### Notes
This experience directly informed the IL risk metrics I built into the Hyperliquid whale tracker. Real pain creates better analytics.

---

## [MEM-002] Used console.log debugging instead of proper error handling in first Solidity project

**Date:** 2026-01-20  
**Context:** Early days of learning Solidity, building ERC-20 token contract  
**Tags:** #solidity #debugging #beginner-mistake #smart-contracts  
**Confidence:** high  
**Status:** active  

### Situation
First real Solidity project. Was debugging a token transfer issue and spent 4 hours using Hardhat's console.log. The bug was a reentrancy vulnerability that console.log couldn't surface because it doesn't show execution flow across contracts.

### Decision
- **Chosen:** console.log debugging (bad choice)
- **Should have done:** Used Foundry's trace output or Tenderly for transaction simulation

### Reasoning
Came from a JavaScript background where console.log is the default debugging approach. Didn't know better tools existed for smart contract debugging.

### Outcome
- **Result:** Negative
- **What happened:** Wasted 4 hours. Finally found the bug by manually reading the code line by line. Later discovered Foundry's -vvvv trace flag would have shown the exact issue in seconds.
- **Timeline:** 4 hours wasted

### Lesson
- Smart contract debugging requires different tools than web debugging. console.log is almost useless for cross-contract issues.
- Foundry's trace flags (-vvvv) should be the first debugging tool, not the last.
- Tenderly simulation is invaluable for understanding transaction flow before deployment.
- When entering a new domain, spend 1 hour learning the debugging tools before writing any code. It saves 10x that time later.
- Related: [[MEM-004]]

---

## [MEM-001] Started with Dune Analytics SQL before trying Python for on-chain data

**Date:** 2026-01-15  
**Context:** Beginning the DeFi analytics journey, needed to query on-chain data  
**Tags:** #analytics #dune #python #data #getting-started  
**Confidence:** medium  
**Status:** active  

### Situation
Wanted to build DeFi analytics dashboards. Had two paths: learn Dune Analytics (SQL-based, web interface) or build custom Python pipelines pulling from RPCs and APIs directly.

### Decision
- **Chosen:** Start with Dune Analytics, add Python later for custom needs
- **Considered:** Python-first approach with web3.py, Flipside Crypto, Nansen
- **Ruled out:** Nansen (paid, not needed for learning), Python-first (too much infrastructure overhead for learning phase)

### Reasoning
Dune lets you query blockchain data with SQL without managing any infrastructure. Faster feedback loop for learning. Can always add Python later for things Dune can't do.

### Outcome
- **Result:** Positive
- **What happened:** Built 3 dashboards in the first week on Dune. Understanding blockchain data structures through SQL made the later transition to Python much smoother because I already understood the data model. The Dune community queries were incredibly helpful for learning patterns.
- **Timeline:** Productive within first 2 days

### Lesson
- For blockchain data analysis, start with Dune to understand the data structures. Add Python/custom pipelines when you hit Dune's limitations.
- Dune limitations that push you to Python: real-time data (Dune has ~10min delay), custom calculations not expressible in SQL, and integrating non-blockchain data sources.
- The community queries on Dune are an underrated learning resource. Search before building from scratch.
- This "learn the domain first with simple tools, build custom later" pattern applies broadly, not just to blockchain data.

### Notes
This lesson shaped my approach to every new tool since. Start with the managed/simple version, understand the domain, then build custom when you know exactly what you need.
