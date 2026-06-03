# claude-config (public)

A sanitized, public snapshot of how I run Claude Code — the **patterns**, not my private brain.

I drive a large Claude Code setup every day: a global `CLAUDE.md`, a routing table of skills,
on-demand context files, hooks, subagents, and a file-based memory. This repo shares the
*architecture* of that system so you can build your own — minus my projects, clients, secrets,
and business specifics.

## What's here

- **`CLAUDE.md`** — a template of the global operating rules (tool autonomy, git, communication,
  capability routing, guardrails, memory). Fill in the `<placeholders>`.
- **`docs/ARCHITECTURE.md`** — the progressive-disclosure pattern: how skills, context files,
  hooks, and memory fit together so the agent loads only what it needs.

## What's deliberately NOT here

This is a *pattern*, not a runnable copy of my setup. Intentionally excluded: any secrets,
`.env` files, OAuth tokens, the actual skills/agents (they name real projects and clients),
session transcripts, traces, and anything business-specific. The `.gitignore` hard-blocks the
dangerous file types so nothing sensitive can land here by accident.

## How to use it

1. Copy `CLAUDE.md` to your `~/.claude/CLAUDE.md` and replace every `<placeholder>`.
2. Read `docs/ARCHITECTURE.md` and adopt the pieces that fit how you work.
3. Build your own skills/context/hooks — start small, add only what earns its place.

## A note on safety (learned the hard way)

If you publish your own config, scan it first. The classic leaks: committed `.env` files,
OAuth `token.json`, scraper cookies, and "backups" of real env files. And remember: your
*traces* (session recordings) can contain secrets that scrolled through tool output — scrub
those separately before sharing any of them.

MIT licensed. Take what's useful.
