# Optimal Claude Code workflow — solo personal projects

## Context

The previous suggestion was a good *skeleton* but incomplete:

| Category | Skills | When |
|---|---|---|
| Personal productivity | `/start` (once), `/update` (regularly) | Independent of project |
| Building a thing | `/product-brainstorming` → `/write-spec` → `/system-design` → build | Per project/feature |

What it misses for someone working the way your CLAUDE.md describes (anti-cognitive-debt, simplicity, architectural pushback, security boundaries):

1. **No decision-recording step.** `/system-design` covers the *shape* of a thing; specific big choices ("Postgres vs SQLite", "FastAPI vs Flask") want `/architecture` (ADR).
2. **No quality gate before merge.** `/review` and `/security-review` exist for this — the second one *directly* implements the "always have a human review crypto/auth" boundary in your CLAUDE.md.
3. **No simplification pass.** `/simplify` is almost a direct expression of your Design Defaults section. Currently invisible in the flow.
4. **No debug/maintenance loop.** `/debug` (structured), `/tech-debt` (periodic), `/documentation` (when it matters).
5. **Memory hygiene is unmentioned.** The auto-memory system silently bloats over time without `/consolidate-memory`.
6. **`/tldr` is missing** — single most useful one-off command for "this answer is too long."
7. **`/start` and `/update` are over-promoted for solo personal use.** Both are built around external task systems (Linear, Slack, calendar) and team shorthand. For solo work with no external source, `/start` is essentially a no-op and `/update` runs against nothing. Demoted to the excluded list.
8. **`init` was misplaced as a *setup* step.** It scans a codebase to write a `CLAUDE.md`. On an empty folder there's nothing to scan. Moved below to "after the first vertical slice lands."
9. **Plan mode at "build" needed clarifying.** It's the *implementation* planner — given a known change, what files to touch and how — not a brainstorming or design tool. Row updated.

---

## The flow

| Phase | Command(s) | Trigger / when |
|---|---|---|
| **Ideation** | `/product-brainstorming` | You have a fuzzy idea and want to think it through before scoping. Skip for obvious tasks. |
| **Scoping** | `/write-spec` | The thing is non-trivial and you want a written target. For one-afternoon hacks, skip and just talk. |
| **Design (shape)** | `/system-design` | Multiple components, data flow, or service boundaries to think about. Skip for single-file changes. |
| **Design (decision)** | `/architecture` | One specific high-stakes choice you want recorded as an ADR (storage, framework, protocol). Often *inside* a design phase. |
| **Build** | Plan mode → normal Claude | Default loop. Plan mode is the *implementation* planner — given a known change, what files to touch and in what order. Distinct from `/product-brainstorming` (idea-level) and `/system-design` (architecture-level). Use for non-trivial slices; skip for renames/typos/one-liners. Aligns with your "ask me my plan first" CLAUDE.md rule. |
| **Capture conventions** | `init` | *After* the first vertical slice lands and the codebase has shape — scans the now-existing code and writes a `CLAUDE.md` so future sessions inherit conventions. Useless on an empty folder. |
| **Pre-merge review** | `/review` | About to merge a PR or finish a feature branch. |
| **Security gate** | `/security-review` | The change touches auth, crypto, input validation, or anything user-supplied. Mandatory per your CLAUDE.md boundaries. |
| **Simplification pass** | `/simplify` | After a feature lands, before you move on. Matches your Design Defaults — catches the speculative abstractions and "just-in-case" knobs. |
| **Debugging** | `/debug` | A bug that isn't 30-seconds-obvious. Forces reproduce → isolate → diagnose, which is exactly the structure that prevents guess-and-hope fixes. |
| **Pre-ship** | `/deploy-checklist` | About to push to anything users (even just you) depend on. Skip for sandbox/scratch projects. |
| **Periodic health** | `/tech-debt` | Every couple of months, or when the codebase starts feeling heavy. Last plan you wrote was effectively this; the skill formalizes it. |
| **Docs (real ones)** | `/documentation` | When writing something a future-you or another person will actually read — README, runbook, API doc. Not for inline comments. |
| **Memory hygiene** | `/consolidate-memory` | Every few weeks once memory exists. Merges duplicates, prunes stale entries. |
| **Verbose-answer escape hatch** | `/tldr` | Anytime my response is more than you need. One-shot, no setup. |

---

## Deliberately excluded (and why)

- `/standup`, `/sprint-planning`, `/roadmap-update`, `/stakeholder-update`, `/metrics-review`, `/synthesize-research`, `/competitive-brief` — built for team/stakeholder workflows. Solo personal: not load-bearing.
- `/testing-strategy` — useful, but for most personal projects "write tests where the logic is tricky" doesn't need a skill. Pull it in *if* you're designing a non-obvious test architecture (integration vs unit, fixtures, parameterization across many cases).
- `/incident-response` — only if you're running something with uptime expectations. Skip until then.
- `/start` — bootstraps `TASKS.md` from existing todos and decodes team shorthand. Solo personal projects with no prior task list and no team jargon: essentially a no-op. Run only if migrating from a real task system.
- `/update` — keep aware of it, but it's primarily for pulling external tasks (Linear, Slack, calendar). Use only if those sources actually drive your work.
- `/loop`, `/schedule`, `/update-config`, `/keybindings-help`, `/fewer-permission-prompts`, `/skill-creator` — admin/automation tools. On-demand, not part of the flow.
- File-format skills (`pdf`, `pptx`, `docx`, `xlsx`) — auto-trigger when the file type appears. Nothing to remember.
- `/claude-api` — only if you're building with the Anthropic SDK directly (not Claude Code itself).

---

## The actual mental model (compressed)

Three loops, nested:

1. **Per-feature loop:** brainstorm → spec → design (+ADR if needed) → build (plan mode for non-trivial slices) → review → security-review (if applicable) → simplify → deploy-checklist (if shipping).
2. **Per-bug loop:** `/debug`.
3. **Per-quarter loop:** `/tech-debt`, `/consolidate-memory`.
4. **Per-repo, once:** `init`, *after* the first vertical slice exists — not before.

Plus two always-available escape hatches: `/tldr` (too much output), plan mode (anywhere a non-trivial change is about to start).

That's it. Everything else is on-demand.

---

## Notes worth keeping in mind

- **Plan mode is the most important "command" here and it isn't one** — it's a mode toggle (Shift+Tab to cycle). For anything non-trivial, entering plan mode before building is the single biggest leverage point and it lines up exactly with your "ask me my plan first" rule.
- **`/security-review` is a hard rule, not a suggestion** — per your CLAUDE.md, auth/crypto/input-validation changes always get reviewed. The skill makes that frictionless; not using it is leaving safety on the table.
- **Don't chain skills mechanically.** Each one is a separate context window; running brainstorm → spec → design back-to-back on a small change is over-process. Pick the *minimum* needed for the size of the work.
- **Memory is silent until it isn't.** It builds up as you work. Running `/consolidate-memory` quarterly is the difference between memory helping you and memory becoming noise.
