# malaysian-harness

## Project overview

The `setup-harness` skill: markdown doctrine + templates that set repos up
for success across five subsystems (instructions, tools, environment, state,
feedback). Kind: CLI-adjacent doc/skill repo — the product is text plus the
check that keeps the text true. Working style: local edits, no deploy.

## Quick setup

```bash
./bin/check    # setup == check here: no deps beyond bash + python3
```

## Hard constraints

- `skills/setup-harness/SKILL.md` ≤ 120 lines — enforced by `bin/check`
- This `AGENTS.md` and `templates/AGENTS.md.tmpl` ≤ 200 lines — enforced by `bin/check`
- Standalone: nothing under `skills/` may reference malaysian-engineering — enforced by `bin/check`
- Archive, never delete: outdated content moves to `archive/` + manifest row — enforced by review
- Doer ≠ grader: only a fresh-context review flips a feature to `verified` — enforced by `bin/check` (verified requires `verified_by`)

## Topic docs

| Topic | Doc |
|---|---|
| Doctrine + decisions log | `principles.md` |
| The skill itself | `skills/setup-harness/SKILL.md` |
| Subsystem standards | `skills/setup-harness/standards/` |
| Per-kind proof + safety | `skills/setup-harness/kinds/` |
| Working styles | `skills/setup-harness/references/working-styles.md` |

## At session start

1. Run `./bin/check` — must be green before any change.
2. Read `Progress.md` — confirm the single active task (1 UP).
3. `git status --short` — note any dirty state before touching files.

## At session end

1. `./bin/check` green, or the failure recorded in `Progress.md`.
2. `Progress.md` + `features.json` updated (builder never sets `verified`).
3. Decisions made this session appended to `principles.md`.
4. Work committed; `git status --short` clean.
5. Toolbelt still consolidated and fast: `bin/check` stays the single entry
   point; new checks fold into it, complexity nested in narrow-and-deep
   helpers — no new top-level script without a recorded reason.
