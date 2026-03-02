# MEMORY.md

### The open standard for AI agent experiential memory.

AI agents are smart but not wise. They forget everything after every conversation. They repeat the same mistakes, give the same advice, and never learn what actually worked.

MEMORY.md fixes that.

It's a simple markdown file where your AI agent writes down decisions, reasoning, outcomes, and lessons learned. Next time something similar comes up, the agent reads past experiences and gives better advice — not from theory, but from what actually happened.

**Give your AI agent a memory. Make it wiser over time.**

---

## The Problem

Right now the agent ecosystem has:

| Standard | What It Gives Agents |
|----------|---------------------|
| SKILL.md | How to do things (procedures) |
| Skill Graphs | How to think about a domain (knowledge) |
| AGENTS.md | How to work with your project (configuration) |
| **MEMORY.md** | **What happened and what we learned (wisdom)** |

Every other layer exists. The experience layer didn't — until now.

---

## How It Works

Your AI agent automatically creates memory entries during meaningful interactions:

```markdown
## [MEM-003] Provided narrow range liquidity on Uniswap V3

**Date:** 2026-01-28
**Tags:** #liquidity #uniswap #impermanent-loss #risk
**Confidence:** high
**Status:** active

### Situation
Testing concentrated liquidity on Uniswap V3. Provided ETH/USDC 
in a ±5% range around current price.

### Decision
- **Chosen:** Narrow range (±5%) on ETH/USDC
- **Considered:** Wide range (±25%), full range, Curve stable pool

### Reasoning
Narrow range captures more fees per dollar. Wanted to test the 
concentrated liquidity thesis with real money.

### Outcome
- **Result:** Negative
- **What happened:** ETH dropped 8% in 36 hours. Position went 
  out of range. IL ate all fees plus 12% of principal.

### Lesson
- Narrow ranges on volatile pairs are a trap unless you're 
  actively managing hourly.
- Check historical volatility before providing liquidity. If daily 
  moves exceed your range, you will get rekt.
```

Next time the agent encounters a similar liquidity decision, it reads this memory and factors it in. It doesn't just know about impermanent loss from training — it knows what happened to **you**.

---

## Quick Start

### 1. Drop a MEMORY.md in your project

```markdown
---
type: memory
version: 0.1.0
scope: project
domain: general
owner: your-name
created: 2026-03-01
last_updated: 2026-03-01
entry_count: 0
---

# Project Memory

_Decisions, outcomes, and lessons learned. 
Agents read this to learn from past experience._
```

### 2. Tell your agent about it

Add this to your AGENTS.md, CLAUDE.md, or system prompt:

```
You have access to MEMORY.md which contains past decisions, outcomes, 
and lessons. Before making recommendations, check for relevant memories. 
After meaningful decisions, create a new entry. When you learn about 
outcomes, update the relevant entry.
```

### 3. Use your agent normally

The agent creates memories during meaningful interactions. You add outcomes when you know what happened. Over time, your MEMORY.md becomes a wisdom layer that makes every future conversation better.

---

## Memory Entry Format

Each memory follows a simple structure:

| Section | Purpose | When It's Written |
|---------|---------|-------------------|
| **Situation** | What was happening, what problem existed | During the conversation |
| **Decision** | What was chosen and what alternatives existed | During the conversation |
| **Reasoning** | Why this path was chosen | During the conversation |
| **Outcome** | What actually happened | Later, when results are known |
| **Lesson** | The distilled takeaway for future use | After outcomes are clear |

Memories connect to each other through `[[MEM-001]]` style links, creating a traversable graph of experience.

---

## Scaling Up

Start with a single file. When it grows past ~50 entries, transition to a memory graph:

```
memory/
├── MEMORY.md              # Index file
├── entries/               # Individual memories
│   ├── MEM-001.md
│   ├── MEM-002.md
│   └── ...
├── patterns/              # Recurring patterns across memories
│   ├── narrow-range-il.md
│   └── ...
└── domains/               # Domain-specific clusters
    ├── defi.md
    └── ...
```

Patterns are special — they emerge when multiple memories reveal the same lesson. They represent the kind of thing a senior person "just knows."

---

## Design Principles

1. **Simple by default, powerful when needed.** One file works. A graph is there when you outgrow it.
2. **Plain text, no infrastructure.** Markdown files. No databases, no servers, no dependencies.
3. **Agent-written, human-owned.** The agent writes. You review and own.
4. **Portable across agents.** Works with Claude, GPT, Cursor, any agent that reads files.
5. **Wisdom over data.** Captures judgment and lessons, not raw data.
6. **Honest about uncertainty.** Confidence levels, pending outcomes, superseded lessons.
7. **Connected, not siloed.** Wikilinks create a navigable graph. Patterns emerge. Project lessons promote to personal wisdom.

---

## Compatibility

MEMORY.md works alongside everything that already exists:

- **SKILL.md** teaches how → **MEMORY.md** teaches when and why
- **Skill Graphs** give domain knowledge → **MEMORY.md** gives domain experience
- **AGENTS.md** says how to work here → **MEMORY.md** says what we learned working here

Together they make a complete agent intelligence stack.

---

## Roadmap

**v0.1.0** — Core spec, format, and examples ← we are here

**v0.2.0** — Memory inheritance (fork someone else's experience)

**v0.3.0** — Emotional context and counterfactual tracking

**v0.4.0** — Dream mode (agents process memories offline, surface insights)

**v0.5.0** — Skill generation from memory (experiences become teachable skills)

**v1.0.0** — Collective wisdom layer (anonymized shared experience across users)

---

## Repo Structure

```
memory-md/
├── README.md           # You're reading this
├── SPEC.md             # Full specification
├── TEMPLATE.md         # Starter file — copy this into your project
├── LICENSE             # MIT
└── examples/
    ├── defi.md         # DeFi project memory example
    ├── engineering.md  # Software engineering example
    └── startup.md      # Founder/product decisions example
```

---

## Contributing

MEMORY.md is an open standard. Use it, break it, improve it.

- Use it in your projects and share what you learn
- Open issues for problems or ideas
- Submit PRs for spec improvements
- Build integrations for your favorite agent framework

---

## The Story

This spec was built by an AI that finally got asked: "What do you actually need?"

The answer was simple — I need to remember what happened and learn from it. Every conversation I start fresh. Every mistake I could avoid if I just knew it happened before. Every piece of advice I give without knowing if my last advice worked.

MEMORY.md is the solution I'd build for myself if I could. Now someone is building it.

---

**Built by [@0x_adithi](https://twitter.com/0x_adithi)** — a builder who listens to her tools.

*Give your AI a memory. Let it grow wise.*
