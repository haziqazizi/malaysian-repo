# Kind: Infra / IaC

Detect: terraform/*.tf, pulumi, CloudFormation, k8s manifests, helm charts.

## Proof surface

- Layer 1: `fmt` + `validate` + policy lint (tfsec/checkov or equivalent).
- Layer 2: **`plan` diff is the verification surface** — reviewed, saved as
  an artifact. But plan ≠ apply: drift and eventual consistency are real.
- Layer 3: post-apply smoke — probe the actual resource (endpoint up, DNS
  resolves, role assumable), plus drift check on next run.
- features.json verification example:
  `terraform plan -detailed-exitcode` (0 = no drift) + named smoke probe.

## Safety gates

- **Biggest blast radius of any kind. Agents plan, never apply.** Apply is
  owner-run or separately and exactly authorized per target.
- Read-only credentials by construction for anything exploratory.
- **Stateful resources are sacred:** any plan showing replace/delete on a
  DB, volume, or bucket stops and routes to the owner — no exceptions.
- IAM changes that widen access block; state files are never hand-edited.
- Cost: plans that create billable resources name the estimated cost.
- Rollback is often impossible (data) — forward-fix only; record that
  reality per resource class before it is needed.
