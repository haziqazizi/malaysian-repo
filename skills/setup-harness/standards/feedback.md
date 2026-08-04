# Standard: Feedback

The subsystem that tells you whether work is actually good — with teeth.

## What

- **Three validation layers**, all wired and kind-shaped (see `../kinds/`):
  1. Syntax/static — parse, lint, type, contract shape
  2. Runtime — tests
  3. System-level — end-to-end proof on the kind's real surface (browser
     evidence for frontend, plan+smoke for infra, eval suite for AI agents…)
  Passing one layer is not "validated".
- **Quality doc** (`QUALITY.md`): per-module letter grade on five fixed
  dimensions — verification passing, agent understandable, test stability,
  architecture boundaries, code conventions. Grades from evidence, not vibes.
- **Every architecture constraint has teeth**: at least one test, lint rule,
  contract, or rubric. Constraint without enforcement = wish.
- **Self-enforcement**: the repo's own `check` fails when any harness
  standard breaks (docs drift, features.json invalid, >1 UP active, …).
- **Friction / papercuts queue.** One append-only log for small pain: a bad
  error message, a slow step, a doc that lied. Entries are **stateless** —
  no status, no owner, no lifecycle. why: lifecycle on papercuts recreates a
  second issue tracker, and a second tracker rots. source: monorepo field
  deployment, 2026-08.
- **A papercut logged this session MUST NOT survive this session as a raw
  log entry.** Before the session ends, each entry you added takes one of
  two exits:
  - **Trivial and safe** — ≤15 minutes, one surface, no schema, money, or
    auth: fix it now and close the entry.
  - **Not trivial** — convert it into a feature row, or a blocked row that
    names its exact gate. Then close the log entry.

  The agent also attempts at least one pre-existing entry that touches the
  surfaces it worked on. why: a future run must never hit a papercut this
  session could have removed. source: monorepo field deployment, 2026-08.
- **Session start and session end are the only cleanup mechanisms.** There
  is no periodic sweep. The end checklist below is therefore sufficient on
  its own. why: cleanup that waits for a schedule never runs.
  source: monorepo field deployment, 2026-08.
- **Session-end blocks.** Every session ends on five blocks (render
  `../templates/session-end.md.tmpl` into the repo):
  1. **The gate** — the verification command passes, or the failure is
     recorded with the exact next action.
  2. **Leave it fast** — compare timings with the recorded bounds. Two warm
     runs over the bound means fix it or log it. Never normalize a slower
     bound silently.
  3. **Leave it startable** — preflight passes, every row state names the
     exact next action, no stray processes or servers left running.
  4. **Leave the map true** — routers list what exists, one owner per fact,
     plain sentences.
  5. **Eliminate this session's papercuts** — every entry you added is
     fixed or converted to a row, and closed. Attempt one pre-existing
     entry on your surfaces. The log ends the session smaller.
- **Weak-model soak.** Acceptance is not the strong model's opinion: a fresh
  **low-capability** agent completes a starter task using only the injected
  docs. Every stumble is logged and fixed. Repeat until a run is clean.
  why: the harness must carry the weakest agent that will use it.
  source: monorepo field deployment, 2026-08.
- Kind-shaped **safety gates** installed per `../kinds/<kind>.md` (migration
  approval, dry-run flags, plan-only creds, protected CI, budget caps…).

## Why

Without layered feedback, "done" means "compiled". Without teeth,
constraints decay into folklore. Without self-enforcement the harness itself
rots — the check that guards the repo must also guard the harness. Small
friction is never urgent, so it is never fixed. A stateless queue that must
be empty of this session's entries at session end keeps the count going
down without a schedule.

## Verify

- Run all three layers this session; each reports pass/fail distinctly.
- Break probe: introduce a temporary violation of one arch constraint and one
  harness rule; `check` must go red. Revert; green.
- QUALITY.md exists and its grades cite evidence (commands, not adjectives).
- The papercuts log exists, appends cleanly, and carries no status column.
- No entry added this session is still open at session end. Each one is
  fixed, or replaced by a feature row (or blocked row with its gate).
- Walk the five session-end blocks this session and record each result.
- Weak-model soak: one fresh low-capability agent, one starter task, docs
  only. Stumbles logged. A clean run is the acceptance evidence.

## Fix

- Missing layer: wire the kind's proof surface per `../kinds/<kind>.md`.
- Missing papercuts log: create it, seed it with the friction found this run,
  then clear this run's entries before the session ends.
- Papercut growing a status field: promote it to a feature row, or fix it now.
- Entry still open at session end: fix it if trivial and safe; otherwise
  write the row, name the gate, and close the entry.
- Timing over bound twice: fix the cause or log it as a papercut — never
  raise the recorded bound to make it pass.
- Toothless constraint: add the cheapest enforcement (usually a grep-based
  lint or a contract test); if genuinely unenforceable, record it as a
  decision with the residual risk named.
- Never weaken a guard to green a number.
