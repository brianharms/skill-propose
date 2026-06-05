---
name: propose
description: Spawn 3 parallel agents to independently propose different approaches to a design question or feature idea, then present them ranked.
---

When invoked, identify the current question, feature idea, or design decision from conversation context. Then launch 3 Agent subagents **in parallel**, each tasked with independently proposing a different approach.

## Agent Instructions

Each agent gets the same context about the question but is assigned a **different design philosophy**:

- **Agent A — "The Pragmatist"**: Propose the most straightforward, minimal-effort solution. Optimize for speed of implementation, simplicity, and low risk. Reuse existing patterns in the codebase. Avoid over-engineering. Ask: "What's the smallest change that gets us there?"
- **Agent B — "The Architect"**: Propose a well-structured, robust solution. Consider extensibility, maintainability, and how this fits into the broader system. Think about edge cases, future requirements, and clean abstractions. Ask: "What's the right way to build this?"
- **Agent C — "The Wildcard"**: Propose something unexpected or creative. Challenge conventional approaches. Consider entirely different frameworks, unconventional UX patterns, novel algorithms, or lateral solutions the others wouldn't think of. Ask: "What if we did it completely differently?"

Each agent must return:
1. **Approach** — What they're proposing and why (2-3 sentences)
2. **How It Works** — Concrete implementation outline: what changes, what gets added, how the pieces connect
3. **Tradeoffs** — Honest pros and cons of this approach
4. **Effort** — Rough scope (Small / Medium / Large)

## Agent Prompts

Give each agent:
- A clear description of the question or goal (what the user wants to achieve or decide)
- The relevant file paths and any context from the conversation
- Their assigned design philosophy (from above)
- Instruction to **read the actual code** and ground their proposal in the real codebase
- Instruction to return their findings in the 4-part format above
- Instruction that they are **research only** — do NOT edit any files

## Presentation

After all 3 agents return, present the results to the user as a ranked list:

### Format

```
## Proposals

### 1. [Title] — Effort: [Small/Medium/Large]
**Approach**: [Pragmatist / Architect / Wildcard]
**What**: ...
**How**: ...
**Tradeoffs**: ...

### 2. [Title] — Effort: [Small/Medium/Large]
...

### 3. [Title] — Effort: [Small/Medium/Large]
...
```

Rank by your own assessment of best fit for the user's situation, weighing simplicity, quality, and creativity. Add a brief note if you think one approach is clearly the best path, or if a hybrid of two approaches would be ideal.

After presenting, ask: **"Want me to go with one of these, or combine ideas?"**
