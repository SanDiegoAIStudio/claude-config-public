# Architecture — progressive disclosure for Claude Code

The whole system is built on one idea: **the agent should load only what it needs, when it
needs it.** Context windows are finite and attention is real. So instead of one giant prompt,
you split knowledge into layers and let the agent pull the right layer on demand.

```
Always loaded (small)          Loaded on demand (large)
─────────────────────          ────────────────────────
~/.claude/CLAUDE.md            skills/<name>/SKILL.md      ← capabilities, pulled by router
memory index (1 line/fact)     context/<domain>.md         ← domain rules, read before working
                               memory/<fact>.md            ← full fact, recalled when relevant
                               hooks                       ← fire on events, not in context
```

## The four layers

**1. Global rules (`CLAUDE.md`) — always on, kept small.**
The non-negotiables: autonomy, package managers, git, communication, guardrails. Plus two
*routers* that point at the heavier layers: a capability table (→ skills) and a context table
(→ domain files). Keep the specifics OUT of here; keep the pointers IN.

**2. Skills — capabilities, pulled by the router.**
Each skill is a folder with a `SKILL.md`: a name, a one-line description used for matching, and
the full instructions. The agent reads the description to decide relevance, then loads the body
only if it's used. This is how you can have 100+ capabilities without 100+ capabilities' worth
of tokens sitting in every prompt. Rule of thumb: if a chance exists that a skill applies,
invoke it; an invoked-but-wrong skill costs little, a missed skill costs a reinvented wheel.

**3. Context files — domain knowledge, read before working.**
Short files (`context/git-workflow.md`, `context/tdd-workflow.md`, …) that the agent reads when
a task enters that domain. Same logic as skills: the router in `CLAUDE.md` says *which file for
which domain*; the file itself loads only when needed.

**4. Memory — facts that survive sessions.**
One fact per file with frontmatter (`name`, `description`, `type`). A tiny index (one line per
fact) is always loaded; the full fact is recalled when its description matches the task. Write
the *non-obvious* (decisions, corrections with their why, project state) — never what the repo
already records.

## Hooks — behavior the model can't do itself

Some things must happen deterministically: format-on-save, a check before every commit, a
nudge on session start. Those are **hooks** — shell commands the harness runs on events
(pre/post tool use, session start, user prompt submit). They're not in the context window;
they're config the runtime executes. Use them for "always do X when Y," because the model
can't reliably remember to.

## Subagents — parallelism and isolation

For independent work, dispatch subagents in parallel (each gets its own context). For risky
or judgment-heavy steps, run several independent passes and reconcile. Route the cheap stuff to
cheap models and the hard stuff to your best one.

## Why it compounds

Every skill, context file, and memory you add makes the *next* task cheaper and more
consistent — without inflating the always-on prompt. The system gets more capable while the
per-task context stays lean. That's the whole game: **breadth in storage, narrowness in
attention.**
