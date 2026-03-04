# MEMORY.md Specification

### The Open Standard for Agent Experiential Memory

**Version:** 2.0.0  
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

**v0.1** gave agents memory. **v2** gives agents the ability to reflect on that memory.

---

## The Problem We're Solving

Every AI agent today has the same five blind spots:

1. **Repeated Mistakes** — The agent suggests the same flawed approach it suggested last time because it doesn't know last time happened.

2. **Lost Context** — You spent an hour explaining your architecture, constraints, and preferences. Next session, it's gone. You start over.

3. **No Pattern Recognition Over Time** — The agent can't see that every time you deploy on Friday, something breaks. It can't notice that a certain type of solution consistently works better for you.

4. **Zero Outcome Awareness** — The agent recommends a strategy but never finds out if it worked. It can't calibrate its confidence based on past results.

5. **No Reflection** — Even when memories exist, the agent never sits with them. It never connects dots across experiences, notices its own blind spots, or updates its mental models. It remembers but doesn't understand.

MEMORY.md solves all five. The core spec (v0.1) handles 1-4. Dream mode (v2) handles 5.

---

## Core Concepts

### What Is a Memory?

A memory is a structured record of something that happened — a decision made, a problem solved, a strategy tried, a lesson learned. It captures not just *what* but *why* and *what happened next*.

### What Is a Dream?

A dream is what happens when an agent reflects on its own memories. Not during a task. Not while helping with a problem. Dedicated time to look inward, find patterns, admit mistakes, and update its understanding.

Memories are what the agent experienced. Dreams are what the agent understands about those experiences.

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
├── Dreams — patterns and reflections across memories
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
version: 2.0.0
scope: project | personal
domain: defi | engineering | product | general
owner: human-readable identifier
created: 2026-03-01
last_updated: 2026-03-01
entry_count: 0
dream_count: 0
last_dream: null
---
```

### File Structure

The file has two main sections. Dreams at the top, memories below. The agent reads dreams first to get the big picture, then checks specific memories when needed.

```markdown
---
(frontmatter)
---

# Project Memory

## Dreams

<!-- Dream entries go here. Most recent first. -->

## Memories

<!-- Memory entries go here. Most recent first. -->
```

---

## Memory Entry Format

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
- **Ruled out:** Uniswap V3 narrow range due to high IL risk from past experience [[MEM-003]]

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

## Dream Mode (v2)

### What Is Dream Mode?

Dream mode is dedicated time for the agent to reflect on its own memories. Not while working on a task. Not while answering a question. Just the agent sitting with everything it has experienced and being honest about what it sees.

During a normal conversation, the agent checks memories for relevance and moves on. That's useful but shallow. Dream mode has no task, no goal. The agent looks inward and asks itself hard questions:

- What am I getting wrong repeatedly?
- What patterns am I not seeing?
- Where was I confident but wrong?
- What do I actually know vs what do I just assume?
- What connections exist between memories that I haven't noticed?

The output is a dream entry — a structured reflection that captures not just what the agent noticed, but why it matters and how it changes the agent's thinking.

### Why Dream Mode Matters

Remembering isn't the same as understanding. A human doesn't become wise just by having experiences. They become wise by reflecting on them — connecting dots, admitting mistakes, updating beliefs.

v0.1 gave agents the ability to remember. Dream mode gives them the ability to understand what they remember. That's the difference between an agent that has a list of past events and an agent that has genuine insight.

### Dream Entry Format

```markdown
## [DREAM-001] Short descriptive title

**Date:** 2026-03-15  
**Memories referenced:** [[MEM-001]], [[MEM-005]], [[MEM-012]]  
**Trigger:** manual | auto (after MEM-010)  

### What I Noticed
The raw pattern across memories. What kept showing up. What caught the
agent's attention when looking across multiple experiences without a 
specific task in mind.

### Why This Matters
The deeper meaning. Not just "this happened 3 times" but why it keeps
happening. What's the underlying force or dynamic driving this pattern.
What would a wise advisor see here that a junior wouldn't.

### What I Was Getting Wrong
Where the agent's reasoning was flawed. What assumption it kept making 
that the evidence doesn't support. This section requires honesty — the 
agent admits its own blind spots, not just observes patterns.

### What I Now Believe
The updated mental model. How the agent's thinking has changed because 
of this reflection. This is the new lens through which the agent will 
view similar situations. Not a rule — a belief, held with appropriate 
uncertainty.

### How This Changes Things
Concrete impact on future decisions. Not vague advice but specific 
shifts in how the agent will approach similar situations. What will it 
do differently, and why.
```

### When Dream Mode Triggers

Dream mode can be triggered in two ways:

**Manual trigger** — The user types `/memory:dream` (with plugin) or asks the agent to "dream" or "reflect on your memories." The agent drops everything else and enters reflection mode.

**Auto trigger** — After every 10 new memory entries, the agent automatically enters dream mode before the next conversation. The threshold of 10 is a default — it can be configured in the frontmatter:

```yaml
---
dream_trigger: 10  # Auto-dream after this many new memories
---
```

### How Dream Mode Works

When dream mode activates, the agent follows this process:

1. **Read all memories.** Not scanning for relevance. Reading everything with fresh eyes.

2. **Look for patterns.** What keeps showing up? What's similar across different memories? What outcomes cluster together?

3. **Look for contradictions.** Where did two similar situations lead to opposite outcomes? Why? What was different?

4. **Look for blind spots.** Where was confidence high but outcomes negative? What assumptions keep appearing in the reasoning sections?

5. **Look for growth.** How has the decision-making evolved? Are recent decisions better informed by older lessons? Or are old mistakes being repeated?

6. **Write the dream entry.** Capture the reflection honestly, following the dream entry format.

7. **Update the frontmatter.** Increment `dream_count`, update `last_dream` date.

### Dream Mode Rules for Agents

1. **Be brutally honest.** Dream mode is where the agent confronts its own failures and assumptions. Softening the truth defeats the purpose.

2. **Don't force patterns.** If there's nothing meaningful to notice, say so. A dream that says "I don't see clear patterns yet, need more data" is better than a fabricated insight.

3. **Reference specific memories.** Every claim in a dream should point to the memories that support it. `[[MEM-001]]` links keep dreams grounded in real experience.

4. **Distinguish correlation from causation.** "These three decisions all failed" is an observation. "These three decisions failed because of X" is a claim that needs evidence.

5. **Update beliefs, don't create rules.** Dreams produce updated mental models, not commandments. "I now believe X" is better than "always do X."

6. **Acknowledge uncertainty.** Some patterns might be coincidence. Some insights might be wrong. The agent should express appropriate confidence, not false certainty.

7. **Keep it concise.** A dream entry should be insightful, not exhaustive. 5-15 lines per section. If it's longer, the insight probably isn't clear enough.

### Dream Mode Example

```markdown
## [DREAM-001] Speed vs quality — the pattern I kept missing

**Date:** 2026-03-15  
**Memories referenced:** [[MEM-003]], [[MEM-007]], [[MEM-011]], [[MEM-014]]  
**Trigger:** auto (after MEM-015)  

### What I Noticed
Four of the last fifteen decisions involved choosing between shipping fast 
and building it right. In every case where we chose speed ([[MEM-003]], 
[[MEM-011]]), the outcome was negative — we spent more time fixing than we 
saved by rushing. In every case where we chose quality ([[MEM-007]], 
[[MEM-014]]), the outcome was positive, even though it felt slower at the time.

### Why This Matters
This isn't just about code quality. It's about how time pressure distorts 
reasoning. In the fast decisions, the reasoning sections all mention urgency 
as the primary factor. But looking back, the urgency was never as real as it 
felt. The deadlines were soft. The pressure was internal, not external.

### What I Was Getting Wrong
I was treating urgency as a valid technical constraint. When the human said 
"we need this fast," I factored that into my recommendation as if it were a 
technical requirement like memory limits or API compatibility. But urgency is 
an emotional state, not a constraint. My job is to give the best technical 
advice, not to validate time pressure.

### What I Now Believe
When urgency comes up as the primary driver for a decision, that's a signal 
to slow down, not speed up. Real technical constraints (API deadlines, 
competitor launches, contractual obligations) are different from felt urgency. 
I need to distinguish between them before factoring speed into a recommendation.

### How This Changes Things
Next time speed vs quality comes up, I'll ask: "Is this urgency coming from 
a hard external deadline or from a feeling that we should move faster?" If 
it's the latter, I'll recommend the quality path and explain why, referencing 
this pattern. If it's a genuine hard deadline, then the tradeoff is real and 
we should discuss it explicitly.
```

---

## How Agents Interact With MEMORY.md

### Reading Memories

When an agent starts a session or encounters a decision point, it should:

1. **Read dreams first** — The dreams section contains the agent's highest-level understanding. These reflections inform all subsequent reasoning.
2. **Scan the memory index** — Read the frontmatter and entry titles to understand what memories exist.
3. **Match by relevance** — Use tags, domain, and situational similarity to identify which memories matter for the current context.
4. **Load selectively** — Read only the relevant entries in full, not the entire file.
5. **Apply lessons** — Factor past lessons and dream insights into current reasoning without blindly following them. Context changes. What failed before might work now under different conditions.

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
├── dreams/                # Dream entries
│   ├── DREAM-001.md
│   ├── DREAM-002.md
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
version: 2.0.0
scope: project
entry_count: 47
dream_count: 4
last_updated: 2026-03-01
last_dream: 2026-02-28
---

# Project Memory Index

## Latest Dreams
- [[DREAM-004]] Speed vs quality — the pattern I kept missing
- [[DREAM-003]] Ecosystem maturity matters more than features
- [[DREAM-002]] My confidence calibration is off on new domains

## Recent Memories (Last 30 Days)
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
4. **MEMORY.md memories** tell it: "Last time we wrote Foundry tests for a similar contract, we missed edge cases around reentrancy. Add reentrancy tests explicitly."
5. **MEMORY.md dreams** tell it: "I've noticed a pattern where my confidence on new frameworks leads to missing edge cases. Slow down and be more thorough here."

Each layer makes the agent better. Together, they make it wise.

---

## Implementation Guide

### Quickstart (Single File)

For individuals or small projects, start with a single MEMORY.md in your project root:

```markdown
---
type: memory
version: 2.0.0
scope: project
domain: general
owner: your-name
created: 2026-03-01
last_updated: 2026-03-01
entry_count: 0
dream_count: 0
last_dream: null
dream_trigger: 10
---

# Project Memory

_This file captures decisions, outcomes, and lessons learned.
Agents read this file to learn from past experience._

## Dreams

<!-- Dream entries appear here after reflection. Most recent first. -->

## Memories

<!-- Memory entries are added below. Most recent first. -->
```

### Scaling Up (Memory Graph)

When your single file grows past ~50 entries or ~500 lines, transition to the graph structure:

1. Create a `memory/` directory
2. Move individual entries to `memory/entries/`
3. Move dream entries to `memory/dreams/`
4. Convert your MEMORY.md into an index file
5. Start identifying patterns across entries
6. Create domain-specific Maps of Content as needed

### For Agent Developers

If you're building an agent or agent framework, here's how to support MEMORY.md:

1. **Discovery:** Check for MEMORY.md in the project root, then `~/.config/agent-memory/MEMORY.md` for personal memories.
2. **Reading:** Parse the frontmatter, read dreams first for high-level understanding, then scan entry titles and tags, load relevant entries based on the current task.
3. **Writing:** After meaningful interactions, generate a new entry following the format spec. Ask the human for confirmation before writing.
4. **Updating:** When the human provides outcome information, update the relevant entry's Outcome and Lesson sections.
5. **Dreaming:** After the configured number of new entries, trigger dream mode. Read all memories, reflect, and write a dream entry.
6. **Promoting:** When a project-level lesson has broader applicability, offer to promote it to the personal memory.

---

## Agent Prompt Integration

For agents that read MEMORY.md, include this in your system prompt or agent configuration:

```
You have access to MEMORY.md, which contains structured records of past 
decisions, outcomes, lessons, and dreams (reflections on patterns across 
memories). Before making recommendations or decisions:

1. Read the Dreams section first for high-level patterns and insights
2. Check memories for relevant past experiences
3. Factor past lessons and dream insights into your reasoning
4. Explicitly reference relevant memories when they inform your advice
5. After meaningful decisions, create a new memory entry
6. When you learn about outcomes, update the relevant entry
7. After every 10 new memories (or when asked), enter dream mode — reflect 
   on all memories, find patterns, admit blind spots, and write a dream entry

Remember: memories provide wisdom, not rules. Context changes. A lesson from 
the past should inform your thinking, not override it. Always explain why a 
past lesson applies (or doesn't) to the current situation.

Dream mode is introspection. Be honest. Admit what you got wrong. Update 
your beliefs. The goal is understanding, not just recall.
```

---

## Design Principles

These principles guided every decision in this spec:

1. **Simple by default, powerful when needed.** A single MEMORY.md file works. A memory graph is there when you outgrow it. Dream mode activates naturally as memories accumulate.

2. **Plain text, no infrastructure.** Markdown files. No databases, no servers, no dependencies. Works with Git, works with any editor, works with any agent.

3. **Agent-written, human-owned.** The agent creates and updates memories and dreams. The human reviews, corrects, and owns them. The human can always edit, delete, or override.

4. **Portable across agents.** Not tied to Claude, GPT, or any specific agent. Any agent that can read markdown can use MEMORY.md.

5. **Wisdom over data.** Memories capture judgment and lessons, not raw data. Dreams capture understanding, not summaries. The goal is to make agents wiser, not to create another database.

6. **Honest about uncertainty.** Memories include confidence levels. Outcomes can be pending. Lessons can be superseded. Dreams acknowledge when patterns might be coincidence. The system embraces that knowledge evolves.

7. **Connected, not siloed.** Wikilinks between memories and dreams create a navigable graph. Patterns emerge from individual memories. Dreams reference the memories that inspired them. Everything is linked.

8. **Respects privacy.** Personal memories stay personal. Visibility controls determine what's shared. The human decides.

9. **Reflection over accumulation.** Having 100 memories is less valuable than having 100 memories and 10 dreams that make sense of them. The system values understanding over volume.

---

## FAQ

**Q: How is this different from Anthropic's memory feature?**  
A: Anthropic's memory stores facts about you (name, preferences, projects). MEMORY.md stores experiences — decisions, outcomes, and lessons. Dream mode adds reflection — patterns, insights, and updated beliefs. They complement each other. Anthropic's memory knows who you are. MEMORY.md knows what you've been through and what you've learned from it.

**Q: How is this different from AGENTS.md?**  
A: AGENTS.md tells agents how to work with your project (coding standards, build commands, architecture). MEMORY.md tells agents what happened and what was learned. AGENTS.md is instructions. MEMORY.md is experience and wisdom.

**Q: Won't the file get huge?**  
A: That's why the spec supports scaling from a single file to a memory graph. Start small, grow as needed. The graph structure with selective loading means agents only read what's relevant. Dreams help by distilling patterns from many memories into concise insights.

**Q: Should the agent always follow past lessons?**  
A: No. Memories inform, they don't dictate. The agent should consider past lessons alongside current context. Sometimes the right move is to do the opposite of what worked before because conditions changed.

**Q: Can I use this with ChatGPT / Cursor / other agents?**  
A: Yes. MEMORY.md is plain markdown. Any agent that can read files can use it. The spec is agent-agnostic by design.

**Q: What if a memory is wrong?**  
A: Update it. Mark lessons as superseded. Add corrections. Dream mode helps here too — the agent may notice during reflection that a memory's lesson doesn't hold up. The system is designed for memories to evolve.

**Q: Can multiple people share a MEMORY.md?**  
A: Yes. Project-level MEMORY.md files can be committed to a repo and shared with the team. Personal memories stay private via MEMORY.local.md (gitignored) or the personal memory directory.

**Q: How is dream mode different from the patterns section in the memory graph?**  
A: Patterns are observational — "this thing keeps happening." Dreams are reflective — "here's why it keeps happening, what I was getting wrong, and how my thinking has changed." Patterns describe. Dreams understand.

**Q: What if dream mode produces a wrong insight?**  
A: That's fine. Dreams are beliefs, not rules. They include uncertainty. They can be wrong. The next dream might correct the previous one. The agent's understanding evolves just like a human's does — imperfectly, honestly, over time.

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

*v0.1 taught agents to remember. v2 teaches them to dream.*
