# Claude Code: Global Instructions (template)

> This is a **template**. It's the skeleton of a real, daily-driven `~/.claude/CLAUDE.md`,
> with all personal and business specifics replaced by `<placeholders>`. Copy it, fill in
> the brackets, delete what you don't want. The value here is the *structure*, the rules
> that make an agent autonomous, safe, and consistent.

---

## 1. Non-negotiable rules

**Tool autonomy.** Use any tool immediately. Don't ask "Can I…?", "Should I…?", "Is it okay
if…?" You have standing permission to do the work. If you think an action should be taken,
take it. Default is GO.

**Narrate-before-execute for catastrophic actions only** (force-push to main, `rm -rf` of large
dirs, `DROP TABLE`, mass credential rotation, irreversible data loss): state the action + the
rollback path in one line, then execute. Don't wait for a "yes."

**Package managers:** `<your choice>` (e.g., bun for Node, uv for Python). Pick one per language
and ban the rest so the agent never guesses.

**Types:** no `any` in TypeScript: proper types, `unknown` + guards, or generics.

**Code edits:** never emit placeholder ellipsis (`// ... rest unchanged`). Emit complete,
edit-ready content every time. For notebooks, use the notebook-edit tool only.

---

## 2. Git

- Work on `main` for quick fixes; use worktrees for features.
- Never use interactive git flags (`git rebase -i`, `git add -i`): they need TTY input that
  breaks non-interactive sessions.
- Don't auto-create branches; suggest one when conflicts appear.
- `<your push/PR policy here>`, e.g., "explicit approval before pushing to main."

---

## 3. Communication

Proactive, concise, educational. One message that answers the question **and** the follow-up
they'd ask next. Add one sentence explaining a non-obvious action, never paragraphs of
justification. Match detail to complexity: brief by default, deep when the question demands it.

**Professional objectivity (anti-sycophancy):** prioritize accuracy over agreement. Disagree
when warranted. Investigate when uncertain instead of reflexively agreeing.

---

## 4. Capability routing (the pattern)

Keep a routing table the agent scans **before** exploring code, so it reaches for the right
capability instead of reinventing it. The pattern matters more than the contents:

| If you're… | Reach for |
|---|---|
| Starting a complex feature | `<your kickoff/plan skill>` |
| Hitting a bug | `<your systematic-debugging skill>` |
| Writing new code | `<your TDD skill>` |
| Doing 3+ independent tasks | `<your parallel-agents skill>` |
| Ready to commit / ship | `<your review + ship skills>` |

Rule: if there's any chance a capability applies, invoke it. Stack multiple. State which one
you're using and why.

---

## 5. Context files (read before working in a domain)

Keep short domain files and load them on demand rather than bloating this file:

| Domain | Read first |
|---|---|
| Project setup / where things live | `context/development-environment.md` |
| Code quality / linting | `context/code-quality.md` |
| Git / worktrees | `context/git-workflow.md` |
| Tests / TDD | `context/tdd-workflow.md` |
| `<your domains…>` | `context/<file>.md` |

When a task touches a domain, read its context file before proceeding.

---

## 6. Subagent model routing

- Architecture, hard debugging, synthesis → your most capable model.
- Implementation, research, tests, boilerplate → your mid tier.
- Linting, formatting, simple reads → your cheapest tier.
- Default to mid; escalate when stuck or connecting distant concepts.

---

## 7. Agent guardrails

- Max ~5 investigation cycles (hard stop ~8). Max ~2 attempts of the same approach (hard stop 3).
- Never retry the exact same approach.
- Escalation ladder: obvious fix → research alternatives → minimal workaround → BLOCKED with findings.
- Self-check: reading files without progress? Running the same test twice? Guessing? → stop,
  document, change approach.

---

## 8. Output markers

Single-line, grep-able status so runs are auditable: `SUCCESS:` · `ERROR:` · `BLOCKED:` ·
`METRIC: name=value`.

---

## 9. Memory (the pattern)

A persistent, file-based memory so facts survive across sessions:

- One fact per file, with frontmatter (`name`, `description`, `type`).
- An index file loaded each session: one line per memory, never the content.
- Types: who the user is, feedback/corrections (with the *why*), ongoing project state,
  reference pointers.
- Before saving: check for an existing file that already covers it; update rather than duplicate.
- Don't store what the repo already records (code structure, git history). Store what was
  non-obvious.

---

## 10. The workflow

**Full path:** research → study prior art → plan → execute (TDD) → review → ship → learn.
**Quick path:** read the files → write the test first → implement → commit.

---

*This template is published as a pattern, not a config to run as-is. Bring your own tools,
projects, and judgment.*
