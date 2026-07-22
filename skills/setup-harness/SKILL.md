---
name: setup-harness
description: "Set a repo up for success — a joy for humans and AI agents to work in. One idempotent command: bootstraps an empty folder, adapts an existing repo, or reconciles a drifted harness. Archives old/confusing content, never deletes. Do NOT use for implementing product features."
version: 1
---

# setup-harness

## Purpose

Install the five-subsystem harness — Instructions, Tools, Environment, State,
Feedback — into a repo, shaped to its project kind(s) and working style.
Missing any subsystem = incomplete harness. Standalone: never make the host
depend on this skill or repo.

## Modes (detected from disk, never asked)

| Signal | Mode | Behavior |
|---|---|---|
| Empty / near-empty folder | **bootstrap** | Scaffold from `templates/` |
| Existing repo, no harness marker | **adapt** | Inventory → map → morph → archive |
| `docs/harness/setup-report.md` present | **reconcile** | Drift audit + repair only |

Same command every time. Idempotent. Re-run is the health check.

## Workflow

1. **Preflight (read-only).** `pwd`, `git status --short`, inventory docs,
   manifests, commands, CI config, bin/ helpers, package scripts. Detect mode.
   Dirty git state: note it; never stash or revert.
2. **Detect kinds and style.** Project kind(s) per package — see `kinds/` —
   and working style — see `references/working-styles.md`. Detect from disk;
   ask only if genuinely ambiguous, one question, with a recommendation.
3. **Walk the five standards.** `standards/{instructions,tools,environment,
   state,feedback}.md`, each judged `met` / `gap` / `blocked` with live
   evidence (a command run, a file read — never docs-say-so). Absent evidence
   is `not verified`, never `met`.
4. **Gap ledger.** Classify each gap `fix-now` / `report-only` / `skip`.
   Skips are recorded decisions with reasons, never omissions. Inventory
   before building: the gap is usually a missing declaration or teardown, not
   an absent system — map onto existing surfaces before adding anything.
5. **Archive pass.** Every doc is **accurate or archived** — no third state.
   Move stale, contradicting, duplicate, or dead content to `archive/` and add
   a row to `archive/MANIFEST.md` (what, why, date). Never delete. Never
   archive code that is imported/executed — only docs, dead scripts, and
   abandoned plans.
6. **Implement fix-now** idempotently, smallest change that satisfies the
   standard. Monorepo file matrix — harness is fractal:

   | Surface | Root | Each relevant subfolder |
   |---|---|---|
   | AGENTS.md | yes — routes down | always (own or pointer) |
   | Check command | yes — whole-repo gate | always (package-scoped) |
   | features.json / Progress.md | yes | only if independently shipped |

7. **Install kind-shaped gates.** Verification surfaces and safety gates per
   `kinds/<kind>.md`. The `verification` in features.json must be the kind's
   real proof surface, not just "tests pass".
8. **Prove live.** Run every command this run claims works. Then acceptance:
   - **Fresh-agent soak** — a fresh agent (never the setup agent; doer ≠
     grader) gets one small real task; every stumble is logged as repo debt.
   - **Day-one test** — a newcomer goes clone → running → tested → knows the
     next task in under 10 minutes, following only AGENTS.md.
9. **Report.** One row per standard (`met` / `fixed` / `skip: <reason>` /
   `blocked`) with evidence, under `docs/harness/setup-report.md`. Fresh
   `git status --short` quoted. Only this run's evidence earns `met`/`fixed`.

## Gates

- **Archive, never delete.** Deletion requests route to the owner.
- **Owner-only decisions block:** deletions, deploy/CI behavior changes,
  secrets, access policy, anything one-way. Reversible assumptions proceed,
  labeled with severity.
- **Live evidence only.** A claim without a command run this session is
  `not verified`.
- **Doer is never the grader.** The soak agent is fresh-context.
- **No self-reference.** Nothing installed in the host may reference or
  depend on this repo. Uninstall must be a no-op for the host.

## Routing

| Situation | Next |
|---|---|
| Product behavior change needed | out of scope — hand back to owner |
| Standards met, deeper polish wanted | re-run soak with a harder task |
| Blocked on owner decision | report with the exact decision needed |
