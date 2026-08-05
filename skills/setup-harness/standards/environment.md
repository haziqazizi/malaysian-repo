# Standard: Environment

The subsystem that makes the repo runnable, provably, before any work starts.

## What

- One-command setup from fresh clone, fast, idempotent.
- An **init checklist** exists (in AGENTS.md "At session start"): explicit
  steps that prove the environment is real — runnable env, test framework
  actually executes, required services reachable. A checklist, not vibes.
- **The readiness surface composes host policy.** The repo's readiness
  command runs the applicable host preflight when one is present, then adds
  the repo's own steps. It never names a provider or a machine. why: the
  host owns machine readiness; a repo that hard-codes one host breaks on
  every other host. source: monorepo field deployment, 2026-08.
- **Environment detection is structural, never name-string guessing.**
  Detect by the Git common directory, marker files, and directory layout —
  not by matching a path, hostname, or repo name. why: name matching passes
  on a copy and fails on a rename. source: monorepo field deployment, 2026-08.
- **Session start is a template, not folklore.** Render
  `../templates/session-start.md.tmpl` into the repo:
  1. Read the local preamble, if one exists.
  2. Run the readiness command and obey its result. Red means stop.
  3. Claim exactly one row, or none.
  4. Resuming a row? Re-run that row's verification first, before editing.
- **Low-on-context protocol.** When context runs low: stop, checkpoint, hand
  off. A checkpoint is a WIP commit, the row updated, and a resume note with
  the exact next action. Never rush the finish on a thin context. why: a
  rushed finish costs the next session more than a clean stop.
  source: monorepo field deployment, 2026-08.
- Required env vars documented without printing secrets.
- Dependencies are installed from locked manifests. source: principles.md
  decision, 2026-08-04. In a monorepo the lock
  files live per package (`api/Gemfile.lock`, `app/pubspec.lock`,
  `web/package-lock.json`, …); a repo root lock file is one valid layout,
  not the required one.

## Why

Everything downstream (state, feedback, verification) assumes a working
environment. An unproven environment converts every later failure into a
which-layer-broke hunt. Session-start proof is cheap; debugging on top of a
broken env is not.

## Verify

- Run the init checklist top to bottom this session; every step passes.
- The readiness command calls the host preflight when one exists, and names
  no provider.
- Environment detection reads a structural signal. Grep for hard-coded
  paths, hostnames, or repo names in the detection code: a hit is a gap.
- Simulate day-one: from a clean state, setup → check completes in under
  10 minutes following only AGENTS.md.
- Test framework probe: run one known-passing test and one deliberately
  failing test; both report correctly.
- **Lock file probe**: every package that declares dependencies has a
  committed lock file **for that package**. Accept per-package lock files;
  do not require one at the repo root.

## Fix

- Write the init checklist into AGENTS.md session-start from the commands
  that actually proved the env this run.
- Readiness command that skips host policy: compose it in, keep the repo
  steps after it.
- Name-string detection: replace with a structural signal (Git common dir,
  marker file).
- Slow/flaky setup: fix or document the wait honestly; never hide a step.
- Missing lock file: generate it with the package manager and commit it in
  its own package directory.
