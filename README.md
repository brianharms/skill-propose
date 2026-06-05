# /propose

> ## ⚠️ Before you start
>
> Installing a Claude Code skill is just copying one folder into `~/.claude/skills/`. Your AI agent can do all of it. **The only human step:** after install, **restart Claude Code** so it picks up the new skill, then type `/propose`.


> 3 parallel agents propose design approaches, ranked.

## What it does

`/propose` is a Claude Code skill for the moment you're stuck on a design decision and want more than one opinion. Invoke it during a conversation and it spins up **three subagents in parallel**, each given a deliberately different design philosophy — a Pragmatist who reaches for the smallest change that works, an Architect who builds for the long haul, and a Wildcard who challenges the premise entirely. Each agent reads your actual codebase, grounds its idea in real files, and reports back in a consistent four-part format (Approach, How It Works, Tradeoffs, Effort). Claude then ranks the three proposals by best fit for your situation, flags a clear winner or a worthwhile hybrid, and asks how you want to proceed.

It's a way to break out of the first-idea trap. Instead of one path forward, you get three honest, code-aware options and a recommendation — in a single turn.

## Install

`/propose` is a single self-contained skill file. Add it to Claude Code by copying the skill folder into your skills directory so that `~/.claude/skills/propose/SKILL.md` exists.

Clone the repo and copy it in:

```bash
git clone https://github.com/brianharms/skill-propose.git
mkdir -p ~/.claude/skills/propose
cp skill-propose/SKILL.md ~/.claude/skills/propose/SKILL.md
```

Or, if you downloaded the repo as a zip, just place `SKILL.md` at `~/.claude/skills/propose/SKILL.md` yourself.

That's it — there's no install script and nothing to build. Once the file is in place, start (or restart) Claude Code and invoke the skill by typing:

```
/propose
```

## Usage

Use `/propose` whenever you're weighing how to build or solve something. You don't need to re-explain the problem if it's already in the conversation — the skill picks up the current question, feature idea, or design decision from context.

Examples:

```
/propose
```

```
/propose how should we handle offline sync for the notes app?
```

```
/propose I want to add theming — what's the best way to structure it?
```

What happens next:

1. Claude identifies the decision at hand from your conversation.
2. It launches three subagents in parallel — Pragmatist, Architect, and Wildcard — each instructed to read the real code and propose (not implement) a solution.
3. When all three return, Claude presents them as a ranked list, each with its approach, implementation outline, honest tradeoffs, and a rough effort estimate (Small / Medium / Large).
4. Claude adds a short note if one path is clearly best or if combining two would be ideal, then asks: **"Want me to go with one of these, or combine ideas?"**

The agents are **research-only** — they explore the codebase and report back, but never edit files. Nothing changes until you pick a direction.

## Requirements / Dependencies

- **Claude Code CLI** — `/propose` is a Claude Code skill and runs inside it.
- **Subagent support** — the skill relies on Claude Code's ability to spawn parallel Agent subagents (the built-in capability used to run the three proposers concurrently). No extra configuration is needed.

There are no companion scripts, no external tools, no MCP servers, and no OS restrictions — this skill is plain Markdown and works anywhere Claude Code runs.

## For AI coding agents

This section is for an AI agent working **on** the skill itself.

**Repo layout:**

```
skill-propose/
├── SKILL.md        # the entire skill — this is the contract
├── LICENSE         # MIT
├── .gitignore
└── README.md
```

**What `SKILL.md` is:** it is the whole product. There is no code, no build step, and no runtime — `SKILL.md` is the prompt-contract that Claude Code reads and executes when a user types `/propose`. Its YAML frontmatter (`name`, `description`) is what makes the skill discoverable and triggerable; the body is the literal instruction set Claude follows. Editing behavior means editing this file's prose, nothing else.

**How to test changes:** install the edited file into `~/.claude/skills/propose/SKILL.md` (overwriting any prior copy), restart Claude Code, then invoke `/propose` mid-conversation on a real design question in a real codebase. Confirm that (a) three subagents actually launch in parallel, (b) each returns the four-part format, and (c) the final output is a ranked list ending in the follow-up question. Test in a project with real source files — the agents are instructed to read actual code, so an empty repo won't exercise the skill properly.

**Invariants — do not break these:**

- **Three agents, run in parallel.** The value of the skill is independent, concurrent exploration. Don't reduce the count or serialize them.
- **The three distinct philosophies must stay distinct** — Pragmatist (smallest change, low risk, reuse existing patterns), Architect (robust, extensible, edge cases), Wildcard (unexpected, lateral, challenges the premise). These are the reason the output is diverse; collapsing or homogenizing them defeats the skill.
- **Agents are research-only.** The instruction that subagents must NOT edit files is load-bearing — `/propose` is a thinking tool, not an implementation tool. Keep that constraint explicit.
- **Agents must read the real codebase.** Proposals are required to be grounded in actual files, not generic advice. Preserve this instruction.
- **Keep the four-part return format** (Approach / How It Works / Tradeoffs / Effort) and the **ranked presentation** that ends with the "Want me to go with one of these, or combine ideas?" prompt. Downstream readers expect this shape.
- **Keep `SKILL.md` self-contained.** Part of the appeal is zero dependencies and a one-file install. Don't introduce scripts, external tools, or required MCP servers without a very good reason — if you do, update the Install and Requirements sections to match.

## License

MIT © 2026 Brian Harms / Ritual Industries ([ritual.industries](https://ritual.industries))
