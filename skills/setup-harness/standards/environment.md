# Standard: Environment

The subsystem that makes the repo runnable, provably, before any work starts.

## What

- One-command setup from fresh clone, fast, idempotent.
- An **init checklist** exists (in AGENTS.md "At session start"): explicit
  steps that prove the environment is real — runnable env, test framework
  actually executes, required services reachable. A checklist, not vibes.
- No init proof → the environment is not treated as ready and work does not
  start.
- Required env vars documented without printing secrets.

## Why

Everything downstream (state, feedback, verification) assumes a working
environment. An unproven environment converts every later failure into a
which-layer-broke hunt. Session-start proof is cheap; debugging on top of a
broken env is not.

## Verify

- Run the init checklist top to bottom this session; every step passes.
- Simulate day-one: from a clean state, setup → check completes in under
  10 minutes following only AGENTS.md.
- Test framework probe: run one known-passing test and one deliberately
  failing test; both report correctly.

## Fix

- Write the init checklist into AGENTS.md session-start from the commands
  that actually proved the env this run.
- Slow/flaky setup: fix or document the wait honestly; never hide a step.
