# claude-config (public)

A sanitized, public snapshot of how I run Claude Code: the **patterns**, not my private brain.

I drive a large Claude Code setup every day: a global `CLAUDE.md`, a routing table of skills,
on-demand context files, hooks, subagents, and a file-based memory. This repo shares the
*architecture* of that system so you can build your own, minus my projects, clients, secrets,
and business specifics.

## What's here

- **`CLAUDE.md`**: a template of the global operating rules (tool autonomy, git, communication,
  capability routing, guardrails, memory). Fill in the `<placeholders>`.
- **`docs/ARCHITECTURE.md`**: the progressive-disclosure pattern: how skills, context files,
  hooks, and memory fit together so the agent loads only what it needs.

## What's deliberately NOT here

This is a *pattern*, not a runnable copy of my setup. Intentionally excluded: any secrets,
`.env` files, OAuth tokens, the actual skills/agents (they name real projects and clients),
session transcripts, traces, and anything business-specific. The `.gitignore` hard-blocks the
dangerous file types so nothing sensitive can land here by accident.

## How to use it

1. Copy `CLAUDE.md` to your `~/.claude/CLAUDE.md` and replace every `<placeholder>`.
2. Read `docs/ARCHITECTURE.md` and adopt the pieces that fit how you work.
3. Build your own skills/context/hooks. Start small, add only what earns its place.

## A note on safety (learned the hard way)

If you publish your own config, scan it first. The classic leaks: committed `.env` files,
OAuth `token.json`, scraper cookies, and "backups" of real env files. And remember: your
*traces* (session recordings) can contain secrets that scrolled through tool output, so scrub
those separately before sharing any of them.

MIT licensed. Take what's useful.

## The story (then and now)

**Then.** My first Claude Code session on record, May 8, 2025, lasted 53 seconds. I didn't come back for real until November 8. From then to December 11 I ran 146 sessions on 28 days: 104,228 messages and 31,847 tool calls.

I kept retyping the same instructions, so they turned into slash commands. On November 14 I asked for a new one called ss: "this will be save state." It still runs in my setup, as /sv.

On November 20 I pasted Anthropic's Claude Code best-practices post and asked for "a round of assesment and fine tuning planning." The audit was blunt. My CLAUDE.md had grown to 291 lines and was "monolithic (all context loaded upfront)", sitting next to 29 slash commands with no namespacing. At 16:25 the whole ~/.claude folder went into git. At 17:06 I fed it more Anthropic engineering posts plus the anthropics/skills repo. By 17:28 a commit titled "Progressive Disclosure, Tool Namespacing, Orchestrators" was in. On Claude Code 2.0.47 and Sonnet 4.5, that meant hand-building conventions out of blog posts, one session at a time.

**Now.** That repo has 724 commits and stays private, because it's wired to my own projects and accounts. This Mac holds 7,716 session log files since September 16. The part that transfers is the gift: claude-config-public, a 133-line rules file in 10 sections (non-negotiables, git, communication, capability routing, context files, subagent routing, guardrails, output markers, memory, workflow) with 5 placeholders. It went public in June 2026, and the October pass made the wording plain.

State: the pattern runs in every session I start. The template is a snapshot and doesn't include my hooks. Copy it to ~/.claude/CLAUDE.md and fill the 5 blanks.
