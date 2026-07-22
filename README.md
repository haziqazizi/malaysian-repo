# malaysian-harness

> One skill, one command: `setup-harness`. It sets a repo up for success — a
> joy for both humans and AI agents to work in. Standalone, small,
> opinionated.

A harness is five subsystems — **Instructions, Tools, Environment, State,
Feedback**. Missing any one = incomplete harness. `setup-harness` installs
all five, shaped to the repo's project kind(s) and working style, and proves
every claim with live evidence.

## What the command does

| You have | Mode | It does |
|---|---|---|
| Empty folder | bootstrap | Scaffold the harness from templates |
| Existing repo | adapt | Inventory → map → morph → archive confusing content |
| Harnessed repo | reconcile | Drift audit + repair |

Detected from disk, never asked. Idempotent — re-running is the health check.
Old or confusing content is **archived with a manifest, never deleted**.

## Layout

```text
skills/setup-harness/
  SKILL.md                    the skill — one screen, ≤120 lines
  standards/                  the five subsystem standards (what/why/verify/fix)
    instructions.md  tools.md  environment.md  state.md  feedback.md
  kinds/                      per-kind proof surfaces + safety gates
    backend.md  frontend.md  cli.md  infra.md  devops.md  ai-agent.md
  references/working-styles.md  how change reaches reality, per style
  templates/                  AGENTS.md, Progress.md, features schema,
                              QUALITY.md, archive manifest
bin/check                     self-enforcement: fails when the harness breaks
principles.md                 the doctrine log this repo is built from
features.json / Progress.md   this repo dogfooding its own state standard
```

## Opinions (the short list)

- Doer is never the grader: builders set `planned`/`active`/`blocked`/`built`;
  only a fresh-context review sets `verified`.
- Live evidence only: a claim without a command run this session is
  `not verified`.
- 1 UP: one feature in-flight; a PR is just its delivery vehicle.
- Every doc accurate or archived — no third state.
- Verification and safety are kind-shaped: browser evidence for frontend,
  plan-never-apply for infra, statistical evals for AI agents.
- Acceptance = a fresh agent survives a real task and a newcomer is
  productive in under 10 minutes.

## Use

Install the skill into your harness's skills directory (symlink
`skills/setup-harness`), then invoke `setup-harness` in the target repo.
Nothing it installs references this repo — uninstalling is a no-op for hosts.

## Development

```bash
./bin/check   # the gate; keep it green
```

Doctrine changes start in `principles.md` (session log), then land in the
skill. Credit where due: several disciplines here are sharpened versions of
ideas from the author's ME system — this repo deliberately does not depend
on it.
