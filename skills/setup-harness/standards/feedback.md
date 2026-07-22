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
- Kind-shaped **safety gates** installed per `../kinds/<kind>.md` (migration
  approval, dry-run flags, plan-only creds, protected CI, budget caps…).

## Why

Without layered feedback, "done" means "compiled". Without teeth,
constraints decay into folklore. Without self-enforcement the harness itself
rots — the check that guards the repo must also guard the harness.

## Verify

- Run all three layers this session; each reports pass/fail distinctly.
- Break probe: introduce a temporary violation of one arch constraint and one
  harness rule; `check` must go red. Revert; green.
- QUALITY.md exists and its grades cite evidence (commands, not adjectives).

## Fix

- Missing layer: wire the kind's proof surface per `../kinds/<kind>.md`.
- Toothless constraint: add the cheapest enforcement (usually a grep-based
  lint or a contract test); if genuinely unenforceable, record it as a
  decision with the residual risk named.
- Never weaken a guard to green a number.
