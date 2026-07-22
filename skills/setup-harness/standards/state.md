# Standard: State

The subsystem that makes progress durable — in files, not chat memory.

## What

- `Progress.md` owns task breakdown. Tasks are non-overlapping or strictly
  sequential. **Default 1 UP**: one feature in-flight at a time; a PR is just
  its delivery vehicle.
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
More than 1 UP hides which change broke what. A prose wishlist can't be
queried, graded, or resumed by the next agent.

## Verify

- `features.json` parses and validates against the schema.
- Count features with state `active`: ≤ 1.
- Progress.md's current task matches the active feature.
- Pick one `verified` feature, run its verification command: passes.

## Fix

- Missing: render `../templates/Progress.md.tmpl` and seed `features.json`
  from the owner's stated next work (≥3 features).
- Multiple actives: owner picks one; rest → `planned` or `blocked`.
