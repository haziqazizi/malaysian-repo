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

## Why

Agents can only act through commands. A missing or interactive command turns
every session into archaeology; an unstable exit code makes verification
unreadable. One obvious place beats five conventions.

## Verify

- Run each command this session. `setup` twice (idempotence). `check` exit
  code 0; then a deliberate break must exit non-zero.
- No command prompts for input when run bare.

## Fix

- Map onto what exists first (package scripts, Makefile, existing bin/) —
  wrap, don't rebuild. Only add a script when nothing native exists.
- Inapplicable (e.g. `dev` for a pure library): record
  `skip: not applicable — <reason>` in the report; never create a no-op.
