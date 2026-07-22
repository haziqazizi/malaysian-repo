# Working Styles — how change reaches reality

Detect before building surfaces: the wrong surface becomes unused ritual.
Axes: **where you edit** (local copy vs the live thing) × **how change
travels** (through gates vs directly) × **substrate reversibility**.

| Style | Change travels by | Rollback | Harness surfaces to install |
|---|---|---|---|
| Deploy to prod (pipeline) | local → CI → artifact → deploy | redeploy previous | full deploy ladder: predeploy, deploy, smoke, rollback commands |
| Work in prod (dev = prod) | edit live thing, restart | backup-before-touch | backup command, small-step discipline, instant logs; **no** deploy ladder theater |
| Trunk + flags | merge fast, ship dark, ramp % | flag flip | flag registry, ramp checklist, kill-switch doc |
| Release trains / versioned artifacts | cut version, users install | ship another version | version/changelog discipline, release checklist, backcompat tests |
| On-prem / client-installed | installer + release notes | customer-side | support matrix doc, upgrade/migration tests |
| GitOps / declarative | edit desired state; reconciler applies | revert the declaration | plan/diff review gate, drift check, no direct prod edits |
| Batch / data | update job in DAG | backfill | backfill runbook, data-quality checks, idempotent jobs |
| Regulated / change-managed | change board, signed artifacts | formal event | audit trail, signed-release tooling, change-record templates |

Detection signals: CI deploy jobs (pipeline), ssh/hotfix notes + no CI
(work-in-prod), flag SDK deps (trunk+flags), release-please/changesets
(trains), terraform+argo/flux (GitOps), DAG frameworks (batch).
Ambiguous → ask once, with a recommendation.
