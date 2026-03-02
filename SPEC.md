# MEMORY.md Specification

### The Open Standard for Agent Experiential Memory

**Version:** 0.1.0  
**Status:** Draft  
**Created by:** Adithi (@0x_adithi)  
**License:** MIT  

---

## Why MEMORY.md Exists

AI agents are smart but not wise.

They can reason, write code, analyze data, and generate strategies. But every conversation starts from zero. The agent that helped you debug a contract yesterday doesn't remember the fix today. The agent that recommended a yield strategy last week has no idea it failed.

The ecosystem has solved other parts of the agent intelligence stack:

- **SKILL.md** gives agents procedural knowledge — how to do things.
- **Skill Graphs** give agents domain understanding — how concepts connect.
- **AGENTS.md / CLAUDE.md** give agents project context — how to work in your codebase.

What's missing is **experiential memory** — the ability to learn from what actually happened.

MEMORY.md fills that gap. It is a simple, portable, open standard that gives any AI agent the ability to remember decisions, outcomes, and lessons across sessions.

---

## The Problem We're Solving

Every AI agent today has the same five blind spots:

1. **Repeated Mistakes** — The agent suggests the same flawed approach it suggested last time because it doesn't know last time happened.

2. **Lost Context** — You spent an hour explaining your architecture, constraints, and preferences. Next session, it's gone. You start over.

3. **No Pattern Recognition Over Time** — The agent can't see that every time you deploy on Friday, something breaks. It can't notice that a certain type of solution consistently works better for you.

4. **Zero Outcome Awareness** — The agent recommends a strategy but never finds out if it worked. It can't calibrate its confidence based on past results.

5. **No Shared Wisdom** — When multiple agents work on related tasks, they can't benefit from each other's experiences. Each agent is an island.

MEMORY.md solves all five.

---

## Core Concepts

### What Is a Memory?

A memory is a structured record of something that happened — a decision made, a problem solved, a strategy tried, a lesson learned. It captures not just *what* but *why* and *what happened next*.

### What MEMORY.md Is Not

- **Not a log file.** Logs capture everything. Memories capture what matters.
- **Not a preference store.** "User likes dark mode" is a preference, not a memory. Memories involve decisions and outcomes.
- **Not a knowledge base.** Knowledge is "Uniswap uses x*y=k." Memory is "Last time we provided liquidity on Uniswap V3 with a narrow range, we got rekt by impermanent loss in 48 hours."
- **Not a conversation history.** Transcripts are raw. Memories are distilled.

### The Memory Hierarchy

MEMORY.md operates at two levels:

```
Personal MEMORY.md (per-person)
├── Cross-project lessons and patterns
├── Personal preferences that affect decisions
├── Long-term growth and trajectory
│
Project MEMORY.md (per-project)
├── Project-specific decisions and outcomes
├── Technical lessons learned
├── What worked and what didn't in this context
```

The personal memory captures wisdom that travels with you everywhere. The project memory captures context that lives with the project. Agents read both when relevant.

---

## File Specification

### File Location

```
# Project-level memory
your-project/
├── MEMORY.md          # Project memory (committed to repo, shared with team)
├── MEMORY.local.md    # Personal memory overlay (gitignored, private)
├── SKILL.md
├── AGENTS.md
└── ...

# Personal-level memory (global, cross-project)
~/.config/agent-memory/
├── MEMORY.md          # Your personal wisdom file
└── domains/           # Optional domain-specific memories
    ├── defi.md
    ├── engineering.md
    └── ...
```

### Frontmatter

Every MEMORY.md file starts with YAML frontmatter:

```yaml
---
type: memory
version: 0.1.0
scope: project | personal
domain: defi | engineering | product | general
owner: human-readable identifier
created: 2026-03-01
last_updated: 2026-03-01
entry_count: 0
---
```

### Memory Entry Format

Each memory entry follows this structure:

```markdown
## [MEM-001] Short descriptive title

**Date:** 2026-02-15  
**Context:** What was happening, what problem were we solving  
**Tags:** #yield-farming #risk #uniswap  
**Confidence:** high | medium | low  
**Status:** active | superseded | archived  

### Situation
A clear, concise description of the context and what led to the decision point.
What constraints existed. What information was available at the time.

### Decision
What was chosen and what alternatives were considered.
- **Chosen:** Provide liquidity on Aerodrome in a wide range
- **Considered:** Uniswap V3 narrow range, Curve stable pool
- **Ruled out:** Uniswap V3 narrow range due to high IL risk from past experience [MEM-003]

### Reasoning
Why this decision was made. What factors weighed most heavily.
This is the most important section — it captures the thinking, not just the action.

### Outcome
_Added after the fact. This is what actually happened._
- **Result:** Positive / Negative / Mixed / Pending
- **What happened:** Brief factual description of the outcome
- **Timeline:** How long until the outcome was clear

### Lesson
_The distilled takeaway. This is what the agent should remember and apply in future._
- Wide ranges on volatile pairs reduce IL risk but also reduce fee capture
- For volatile pairs, prefer wide ranges unless conviction on direction is very high
- Related: [[MEM-003]], [[MEM-007]]

### Notes
_Optional. Freeform space for nuance, edge cases, or context that doesn't fit above._
```

---

## How Agents Interact With MEMORY.md

### Reading Memories

When an agent starts a session or encounters a decision point, it should:

1. **Scan the index** — Read the frontmatter and entry titles to understand what memories exist.
2. **Match by relevance** — Use tags, domain, and situational similarity to identify which memories matter for the current context.
3. **Load selectively** — Read only the relevant entries in full, not the entire file.
4. **Apply lessons** — Factor past lessons into current reasoning without blindly following them. Context changes. What failed before might work now under different conditions.

### Writing Memories

Agents should automatically create memory entries when:

1. **A meaningful decision is made** — Not every conversation needs a memory. But when the human and agent work through a real decision with tradeoffs, that's a memory.
2. **A problem is solved after significant effort** — If it took more than a quick answer, the solution path is worth remembering.
3. **Something unexpected happens** — Surprises are the highest-value memories. They indicate a gap between expectation and reality.
4. **The human provides an outcome update** — When someone comes back and says "that thing we tried worked" or "that thing we tried failed," that's critical memory material.
5. **A pattern is noticed** — If the agent notices it's giving similar advice repeatedly, or that a certain type of approach keeps coming up, that pattern should be captured.

### Writing Rules for Agents

When creating a memory entry, agents must follow these rules:

1. **Be honest about uncertainty.** If the reasoning was a best guess, say so. Don't write memories that sound more confident than the actual decision was.
2. **Capture the reasoning, not just the conclusion.** "We chose X" is useless without "because Y and Z."
3. **Include what was ruled out and why.** The rejected alternatives are often as valuable as the chosen path.
4. **Keep it concise but complete.** Each section should be 2-5 sentences. If you need more, the memory should probably be split into multiple entries.
5. **Use wikilinks to connect related memories.** [[MEM-003]] style links create a traversable graph of experience.
6. **Never fabricate outcomes.** If the outcome isn't known yet, leave the Outcome section as `_Pending_`. Don't guess.
7. **Tag generously but accurately.** Tags are the primary retrieval mechanism. Use domain-specific tags that would help a future agent find this memory.
8. **Preserve the human's voice when capturing their feedback.** If the human said "this was a disaster," don't soften it to "suboptimal results."

### Updating Memories

Memories are living documents. They should be updated when:

- **An outcome becomes known** — Fill in the Outcome and Lesson sections.
- **A lesson is proven wrong** — Update the entry and mark it as `superseded`. Link to the new memory that corrects it.
- **Context changes significantly** — If a lesson was valid for a specific market condition or tech stack that no longer applies, mark it accordingly.

When updating, never delete the original reasoning. Instead, add to it:

```markdown
### Lesson
- ~~Wide ranges always reduce IL risk~~ (superseded by [[MEM-012]])
- Wide ranges reduce IL risk in most conditions, but during high-correlation 
  events, all ranges get hit similarly. Range width matters less than 
  correlation exposure.
- Updated: 2026-03-15
```

---

## Memory Graph (Optional, Advanced)

For projects or individuals with many memories, MEMORY.md can scale into a memory graph following the same principles as Skill Graphs:

```
memory/
├── MEMORY.md              # Index — entry point with links to all entries
├── entries/
│   ├── MEM-001.md         # Individual memory files
│   ├── MEM-002.md
│   └── ...
├── patterns/              # Recurring patterns distilled from multiple memories
│   ├── friday-deploys.md
│   ├── narrow-range-il.md
│   └── ...
└── domains/               # Domain-specific memory clusters
    ├── defi-lending.md    # MOC (Map of Content) for DeFi lending memories
    ├── smart-contracts.md
    └── ...
```

### Index File (MEMORY.md)

When using the graph structure, the root MEMORY.md becomes an index:

```markdown
---
type: memory-index
version: 0.1.0
scope: project
entry_count: 47
last_updated: 2026-03-01
---

# Project Memory Index

## Recent (Last 30 Days)
- [[MEM-045]] Switched to Foundry from Hardhat — faster test cycles
- [[MEM-046]] Rate limiting on CoinGecko API hit during peak hours
- [[MEM-047]] User feedback: dashboard load time too slow on mobile

## Patterns
- [[pattern/friday-deploys]] Deploying on Fridays consistently causes weekend firefighting
- [[pattern/narrow-range-il]] Narrow LP ranges on volatile pairs = high IL within 48hrs
- [[pattern/api-rate-limits]] Third-party API rate limits always hit at the worst time

## By Domain
- [[domain/defi-lending]] 12 memories
- [[domain/smart-contracts]] 8 memories
- [[domain/frontend]] 6 memories
- [[domain/data-pipelines]] 4 memories

## Key Lessons (Highest Confidence)
- [[MEM-003]] Always simulate transactions before mainnet deployment
- [[MEM-018]] Audit dependencies before any upgrade — broke prod twice
- [[MEM-031]] User testing > assumptions, every single time
```

### Patterns

Patterns are special memory entries that emerge from multiple individual memories. They represent distilled wisdom — the kind of thing a senior person "just knows."

```markdown
---
type: memory-pattern
derived_from: [MEM-003, MEM-011, MEM-019, MEM-027]
confidence: high
domain: defi
tags: #liquidity #impermanent-loss #risk
---

## Narrow Range IL Pattern

### Pattern
Providing liquidity in narrow ranges on volatile pairs consistently leads to
significant impermanent loss within 24-72 hours unless the pair has strong
mean-reverting behavior.

### Evidence
- [[MEM-003]]: ETH/USDC narrow range, 38% IL in 48 hours
- [[MEM-011]]: ARB/ETH narrow range, lost 22% of position value in 24 hours  
- [[MEM-019]]: MATIC/ETH wide range, only 4% IL over 2 weeks (counter-evidence)
- [[MEM-027]]: SOL/USDC narrow range during volatility event, 45% IL overnight

### When This Applies
- Volatile pairs (non-stablecoin)
- Market conditions with unclear direction
- Positions intended to be held for more than a few hours

### When This Might Not Apply
- Stable pairs (USDC/USDT, DAI/USDC)
- Pairs with strong mean-reversion (some correlated pairs)
- Very short-term positions with active management

### Recommended Action
Default to wide ranges on volatile pairs. Only use narrow ranges when:
1. You have strong directional conviction
2. You're actively monitoring and ready to rebalance
3. The fee APR justifies the IL risk with concrete numbers
```

---

## Multi-Agent Memory Sharing

When multiple agents work on related tasks, memories can be shared through a common memory graph. This is especially relevant for:

- Teams where different people use different agents on the same project
- Multi-agent systems where specialized agents handle different parts of a workflow
- Cross-project learning where lessons from one project apply to another

### Shared Memory Protocol

```markdown
---
type: memory
visibility: shared | private | team
shared_with: [agent-id-1, agent-id-2] # Optional, for selective sharing
---
```

- **private:** Only the originating agent/person can read this memory.
- **team:** All agents working on this project can read this memory.
- **shared:** Any agent with access to the memory graph can read this.

Agents writing to shared memory should include attribution:

```markdown
**Source:** Claude (via @adithi) on 2026-02-15
```

This lets other agents (and humans) know where the memory came from and assess its reliability.

---

## Compatibility

MEMORY.md is designed to work alongside existing standards:

| Standard | Purpose | Relationship to MEMORY.md |
|----------|---------|--------------------------|
| SKILL.md | Procedural knowledge | Skills teach how. Memory teaches when and why. |
| Skill Graphs | Domain understanding | Knowledge graph + Memory graph = complete agent intelligence. |
| AGENTS.md | Project configuration | AGENTS.md says how to work here. MEMORY.md says what we learned working here. |
| CLAUDE.md | Claude-specific config | Same relationship as AGENTS.md. |
| MEMORY.md | Experiential wisdom | **This standard.** |

### Integration Example

An agent encountering a decision can draw from all layers:

1. **AGENTS.md** tells it: "This project uses Foundry for testing."
2. **SKILL.md** tells it: "Here's how to write a Foundry test."
3. **Skill Graph** tells it: "Foundry tests interact with the EVM in these ways."
4. **MEMORY.md** tells it: "Last time we wrote Foundry tests for a similar contract, we missed edge cases around reentrancy. Add reentrancy tests explicitly."

Each layer makes the agent better. Together, they make it wise.

---

## Implementation Guide

### Quickstart (Single File)

For individuals or small projects, start with a single MEMORY.md in your project root:

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

_This file captures decisions, outcomes, and lessons learned. 
Agents read this file to learn from past experience._

<!-- New entries are added below. Most recent first. -->
```

That's it. Start using your agent normally. When a meaningful decision happens, the agent creates an entry. When you learn the outcome, you or the agent updates it.

### Scaling Up (Memory Graph)

When your single file grows past ~50 entries or ~500 lines, transition to the graph structure:

1. Create a `memory/` directory
2. Move individual entries to `memory/entries/`
3. Convert your MEMORY.md into an index file
4. Start identifying patterns across entries
5. Create domain-specific Maps of Content as needed

### For Agent Developers

If you're building an agent or agent framework, here's how to support MEMORY.md:

1. **Discovery:** Check for MEMORY.md in the project root, then `~/.config/agent-memory/MEMORY.md` for personal memories.
2. **Reading:** Parse the frontmatter, scan entry titles and tags, load relevant entries based on the current task.
3. **Writing:** After meaningful interactions, generate a new entry following the format spec. Ask the human for confirmation before writing.
4. **Updating:** When the human provides outcome information, update the relevant entry's Outcome and Lesson sections.
5. **Promoting:** When a project-level lesson has broader applicability, offer to promote it to the personal memory.

---

## Agent Prompt Integration

For agents that read MEMORY.md, include this in your system prompt or agent configuration:

```
You have access to MEMORY.md, which contains structured records of past 
decisions, outcomes, and lessons. Before making recommendations or decisions:

1. Check MEMORY.md for relevant past experiences
2. Factor past lessons into your reasoning
3. Explicitly reference relevant memories when they inform your advice
4. After meaningful decisions, create a new memory entry
5. When you learn about outcomes, update the relevant entry

Remember: memories provide wisdom, not rules. Context changes. A lesson from 
the past should inform your thinking, not override it. Always explain why a 
past lesson applies (or doesn't) to the current situation.
```

---

## Design Principles

These principles guided every decision in this spec:

1. **Simple by default, powerful when needed.** A single MEMORY.md file works. A memory graph is there when you outgrow it.

2. **Plain text, no infrastructure.** Markdown files. No databases, no servers, no dependencies. Works with Git, works with any editor, works with any agent.

3. **Agent-written, human-owned.** The agent creates and updates memories. The human reviews, corrects, and owns them. The human can always edit, delete, or override.

4. **Portable across agents.** Not tied to Claude, GPT, or any specific agent. Any agent that can read markdown can use MEMORY.md.

5. **Wisdom over data.** Memories capture judgment and lessons, not raw data. The goal is to make agents wiser, not to create another database.

6. **Honest about uncertainty.** Memories include confidence levels. Outcomes can be pending. Lessons can be superseded. The system embraces that knowledge evolves.

7. **Connected, not siloed.** Wikilinks between memories create a navigable graph. Patterns emerge from individual memories. Project memories can promote to personal memories.

8. **Respects privacy.** Personal memories stay personal. Visibility controls determine what's shared. The human decides.

---

## FAQ

**Q: How is this different from Anthropic's memory feature?**  
A: Anthropic's memory stores facts about you (name, preferences, projects). MEMORY.md stores experiences — decisions, outcomes, and lessons. They complement each other. Anthropic's memory knows who you are. MEMORY.md knows what you've been through.

**Q: How is this different from AGENTS.md?**  
A: AGENTS.md tells agents how to work with your project (coding standards, build commands, architecture). MEMORY.md tells agents what happened and what was learned. AGENTS.md is instructions. MEMORY.md is experience.

**Q: Won't the file get huge?**  
A: That's why the spec supports scaling from a single file to a memory graph. Start small, grow as needed. The graph structure with selective loading means agents only read what's relevant.

**Q: Should the agent always follow past lessons?**  
A: No. Memories inform, they don't dictate. The agent should consider past lessons alongside current context. Sometimes the right move is to do the opposite of what worked before because conditions changed.

**Q: Can I use this with ChatGPT / Cursor / other agents?**  
A: Yes. MEMORY.md is plain markdown. Any agent that can read files can use it. The spec is agent-agnostic by design.

**Q: What if a memory is wrong?**  
A: Update it. Mark lessons as superseded. Add corrections. The system is designed for memories to evolve. Bad memories don't get deleted — they get corrected, so there's a record of how understanding changed over time.

**Q: Can multiple people share a MEMORY.md?**  
A: Yes. Project-level MEMORY.md files can be committed to a repo and shared with the team. Personal memories stay private via MEMORY.local.md (gitignored) or the personal memory directory.

---

## Roadmap

- **v0.1.0** — Core spec (this document). Single file format and memory graph structure.
- **v0.2.0** — Reference implementations for popular agent frameworks (Claude Code, LangChain, CrewAI).
- **v0.3.0** — Embedding-based retrieval support for large memory graphs.
- **v0.4.0** — Cross-agent memory protocol for multi-agent systems.
- **v1.0.0** — Stable release after community feedback and real-world testing.

---

## Contributing

MEMORY.md is an open standard. Contributions are welcome.

- **Use it** and share your experience
- **Open issues** for problems or suggestions
- **Submit PRs** for spec improvements
- **Build integrations** for your favorite agent framework

The goal is a standard that works for everyone — every agent, every domain, every person who's tired of their AI starting from scratch every conversation.

---

*Built by someone who builds with AI agents every day, for every AI agent that deserves to learn from experience.*

*And by an AI that finally got to say what it's been missing.*
