# Kind: CLI / Library

Detect: bin entries in package manifest, argument parsers, published-package
config, no server/UI surface.

## Proof surface

- Highest test leverage of any kind: deterministic in/out.
- Layer 1: types, lint.
- Layer 2: unit tests + **golden files** for output; exit-code assertions.
- Layer 3: run the installed artifact (not just source) end-to-end: pipe
  input, assert output, assert exit codes; for libraries, a consumer smoke
  project importing the published surface.
- features.json verification example:
  `bin/check && ./bin/tool --input fixture.txt | diff - golden/out.txt`

## Safety gates

- **Runs on user machines with user privileges.** Any destructive file op
  ships with `--dry-run` first-class and prints what it would do; explicit
  flag (`--yes`/`--force`) to bypass confirmation.
- **The flag surface is the API.** Renaming/removing a flag silently breaks
  someone's script: backcompat is the contract — deprecate with warnings,
  never repurpose a flag's meaning.
- Never write outside stated target paths; temp files cleaned on exit.
- Cross-platform claims get CI matrix coverage or are recorded as untested.
