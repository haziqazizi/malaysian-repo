# ExecPlan — setup-harness scaffold

Context: `docs/skill-outputs/me-lfg/lfg-20260722-setup-harness-scaffold-context.md`
Status: active

## Principles selected (converted to actions)

- one-source-of-truth → doctrine lives in `skills/setup-harness/`; `principles.md` stays the session log, not a second copy of the skill.
- pit-of-success / boring-by-default → plain markdown + shell; no framework, no deps beyond python3 for JSON parse.
- prove-it-works → `bin/check` must pass AND fail correctly (negative probe).
- minimize-reader-load → SKILL.md one screen (≤120 lines); depth in standards/kinds files.
- name-residual-risk → residuals listed in closeout (no remote, soak untested on a real repo).
- doer-is-never-grader → fresh subagent review before commit.

## Units

1. [x] Context + plan + DONE contract materialized
2. [ ] Scaffold: SKILL.md, standards/ (5), kinds/ (6), references/working-styles.md, templates/ (5)
3. [ ] Repo surfaces: README.md, AGENTS.md, bin/check, root features.json + Progress.md (dogfood)
4. [ ] Prove: `bin/check` green; negative probe red; features.json validates
5. [ ] Fresh review (subagent, refuting stance) → fix findings → re-check
6. [ ] git init + commit
7. [ ] Closeout: statuses, evidence, residuals, resume gate

Resume gate if interrupted: re-read context, `git status --short`, re-run `bin/check`, continue at first unchecked unit.
