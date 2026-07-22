# Review — setup-harness scaffold

Run: lfg-20260722-setup-harness-scaffold · Reviewer: fresh-context subagent
a0c417dad9fc25002 (refuting stance, not the builder) · Date: 2026-07-22

**Verdict: PASS** — DONE criteria 1–7 each verified by the reviewer with
commands/reads. 0 blocking, 2 should-fix, 4 nits.

## Findings and disposition

| # | Severity | Finding | Disposition |
|---|---|---|---|
| 1 | should-fix | bin/check existence-checked QUALITY.md.tmpl / Progress.md.tmpl only; gutting their sections stayed green | fixed — 5b greps dimensions + sections |
| 2 | should-fix | first-real-commissioning verification was prose, not a command | fixed — parameterized runnable command |
| 3 | nit | install-script verification references not-yet-existing install.sh | accepted — command comes into existence with the planned feature |
| 4 | nit | "exactly 5 standards" not enforced against a stray 6th file | fixed — 5c counts standards (5) and kinds (6) |
| 5 | nit | Progress.md active vs features.json built mismatch during review window | resolved — features flipped to verified post-review; Progress.md updated |
| 6 | nit | README omitted `planned` from builder-settable states; vacuous-pass edge in self-check-teeth probe | README fixed; probe edge accepted (unreachable after green check) |

## Reviewer evidence (verbatim highlights)

- `./bin/check` → green, exit 0; also green from `/tmp` (run-from-anywhere).
- `wc -l`: SKILL.md 86 ≤ 120; AGENTS.md 45 ≤ 200; AGENTS.md.tmpl 51 ≤ 200.
- Standalone grep over `skills/` → no ME references (exit 1).
- Negative probes: missing-file → RED with actionable message; 2-active →
  `FAIL: 2 features active — default is 1 UP`; both restored byte-identical,
  final green.
- Full-read cross-check against principles.md: no contradictions found
  (monorepo matrix, 1 UP, state machine, kind matrix, 8 working styles,
  AGENTS.md sections, quality dimensions all faithful).

Post-fix `./bin/check`: green (coordinator run, see closeout).
