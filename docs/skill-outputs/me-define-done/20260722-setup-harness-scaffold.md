# DONE — setup-harness scaffold

Run: lfg-20260722-setup-harness-scaffold
Scope: scaffold the skill + repo self-surfaces. Out of scope: running
setup-harness against a real target repo, remote/PR, install tooling.

| # | Criterion | Verification |
|---|---|---|
| 1 | `skills/setup-harness/SKILL.md` exists: frontmatter (name, description), 3 modes (bootstrap/adapt/reconcile), 5-subsystem walk, gap ledger (fix-now/report-only/skip-as-decision), archive-never-delete, live-evidence rule, fresh-agent soak, monorepo matrix, ≤120 lines | `bin/check` (existence, greps, line cap) + review read |
| 2 | `standards/` holds exactly 5 files (instructions, tools, environment, state, feedback), each with What/Why/Verify/Fix sections | `bin/check` |
| 3 | `kinds/` holds 6 files (backend, frontend, cli, infra, devops, ai-agent), each with Proof surface + Safety gates | `bin/check` |
| 4 | `templates/`: AGENTS.md.tmpl (≤200 lines, 6 required sections), Progress.md.tmpl, features.schema.json (valid JSON; requires behavior/verification/state), QUALITY.md.tmpl (5 dimensions), ARCHIVE-MANIFEST.md.tmpl | `bin/check` incl. JSON parse + line cap |
| 5 | `bin/check` executable, exits 0 on healthy repo, exits non-zero with actionable message when a required file is missing | run it; negative probe (temp-move a file) |
| 6 | Standalone: no path/install dependency on malaysian-engineering anywhere under `skills/` | `bin/check` grep |
| 7 | Repo dogfoods itself: root `features.json` (validates against own schema), `Progress.md` (1 UP), `README.md`, `AGENTS.md` (≤200 lines, routes) | `bin/check` + python3 schema-shape validation |
| 8 | Fresh-context review (refuting stance) returns pass | review artifact |
| 9 | Git repo initialized; all work committed with clean status | `git status --short` empty; `git log` |

Builder may mark criteria active/blocked, never passing — only the fresh
review flips to pass (criterion 8 gates the rest).
