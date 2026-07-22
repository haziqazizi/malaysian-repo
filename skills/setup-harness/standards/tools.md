# Standard: Tools

The subsystem of commands the agent (and human) calls to operate the repo.

## What

Four canonical commands exist, discoverable in one obvious place (`bin/` or
package scripts), named in AGENTS.md Quick setup:

| Command | Contract |
|---|---|
| setup | fresh clone → working environment, idempotent |
| check | the canonical verification gate; exits non-zero on failure |
| dev | starts the thing with predictable logs (kinds that run) |
| logs | shows what the running system says without guessing |

All four: non-interactive by default, stable exit codes, actionable errors.
Monorepo: `check` exists package-scoped in every relevant subfolder plus a
whole-repo gate at root.

The four commands are the whole interface — **narrow and deep**: entry
points stay few, small, and fast; complexity nests inside helper
modules/scripts, invisible from the surface. New capability folds into an
existing entry point (flag or subcommand), not a fifth top-level script —
adding one requires a recorded decision. Slow checks are debt: the next
agent pays them every session start.

## Why

Agents can only act through commands. A missing or interactive command turns
every session into archaeology; an unstable exit code makes verification
unreadable. One obvious place beats five conventions.

## Verify

- Run each command this session. `setup` twice (idempotence). `check` exit
  code 0; then a deliberate break must exit non-zero.
- No command prompts for input when run bare.
- Count top-level entry points: more than the canonical four needs a
  recorded decision naming each extra. Time `check`; if it exceeds ~2
  minutes, that is a gap (fast path or fix), not a fact of life.

## Fix

- Map onto what exists first (package scripts, Makefile, existing bin/) —
  wrap, don't rebuild. Only add a script when nothing native exists.
- Inapplicable (e.g. `dev` for a pure library): record
  `skip: not applicable — <reason>` in the report; never create a no-op.
