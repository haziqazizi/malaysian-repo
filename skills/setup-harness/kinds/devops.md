# Kind: DevOps / CI/CD

Detect: .github/workflows, .gitlab-ci.yml, Jenkinsfile, deploy scripts,
release tooling.

## Proof surface

- Layer 1: workflow-file lint/schema check (actionlint or equivalent).
- Layer 2: local approximation where possible (act, script extraction — CI
  steps call the same `bin/` scripts the developer runs, so the logic is
  testable outside the pipeline).
- Layer 3: **the pipeline only truly runs in the pipeline env** — branch
  dry-runs: push to a test branch, observe the real run before touching the
  main workflow.
- features.json verification example: link to a green run URL of the changed
  workflow on a branch, plus `bin/check`.

## Safety gates

- **Meta-risk: a CI change modifies the gate that verifies everything else.**
  Protect the verifier — changes to check/gate workflows get their own
  review; never let a change disable the check that would have caught it.
- Secrets never echo to logs; add a log-grep probe for known secret shapes.
- **Pin third-party actions/images by SHA** — unpinned supply chain is a gap.
- Deploy credentials live only in the pipeline env, never in repo files.
- CI must run the same check the developer runs, verbatim — divergence is
  how "works locally" ships broken.
