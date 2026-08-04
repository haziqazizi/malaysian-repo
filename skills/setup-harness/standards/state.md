# Standard: State

The subsystem that makes progress durable — in files, not chat memory.

## What

- **Survivorship rule.** A row is required exactly when the work must
  outlive the session that conceived it: multi-session, blocked, risky, or
  touched by more than one worker. Reading, investigation, and trivial fixes
  finished in one sitting need no row. why: rows for throwaway work make the
  list untrustworthy, and an untrusted list stops being read.
  source: monorepo field deployment, 2026-08.
- Task breakdown is durable state. `Progress.md` owns it in a single-worker
  repo. Tasks are non-overlapping or strictly sequential. **Default 1 UP**:
  one feature in-flight at a time; a PR is just its delivery vehicle.
- **Fleet scale.** With several agents working at once, WIP = 1 **per
  worker**, not one for the whole repo. Each worker owns exactly one active
  row and names itself as the owner. why: a global WIP of 1 serializes a
  fleet and workers then keep state in chat. source: monorepo field
  deployment, 2026-08.
- **The function is required, the filename is not.** Durable state must
  answer three questions: current state, next steps, blockers. In a
  multi-worker repo, per-row state inside `features.json` (owner, status,
  next action, blocker) is an accepted substitute for `Progress.md`. One
  shared prose file is a write-conflict surface.
- `features.json` is a machine-readable state machine validating against
  `../templates/features.schema.json`. Each feature: **behavior** (what it
  does), **verification** (the command that proves it, kind-shaped),
  **state**.
- State transitions respect doer ≠ grader: a builder may set
  `active` / `blocked` / `built` — only a fresh-context review sets
  `verified`.
- Long operations are resumable: interrupted work leaves enough on disk
  (progress notes, exact resume step) that a fresh session continues without
  re-derivation.
- Monorepo: features.json / Progress.md per subfolder only if it ships
  independently; otherwise root-only (state fragments → 1 UP silently
  becomes N UP).

## Why

Chat memory dies with the session. Overlapping tasks corrupt each other.
More than 1 UP per worker hides which change broke what. A prose wishlist
can't be queried, graded, or resumed by the next agent. A list that also
records single-sitting work becomes noise, and noise is skipped.

## Verify

- `features.json` parses and validates against the schema.
- Count features with state `active` per owner: ≤ 1 each. A repo with one
  worker keeps the global count at ≤ 1.
- Durable state answers current state, next steps, and blockers. Accept
  either form: `Progress.md`, or per-row state in `features.json`. Absence
  of `Progress.md` alone is not a gap.
- Every active row names an exact next action.
- Pick one `verified` feature, run its verification command: passes.

## Fix

- Missing: render `../templates/Progress.md.tmpl` and seed `features.json`
  from the owner's stated next work (≥3 features). In a multi-worker repo,
  put the same fields in the rows instead.
- Multiple actives for one owner: that owner picks one; rest → `planned` or
  `blocked`.
- Rows for single-sitting work: close or drop them; keep the survivorship
  rule.
