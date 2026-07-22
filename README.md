# malaysian-harness

**Reliable execution toward a task — by humans or AIs.**

Strong models don't mean reliable execution. As of late 2025, the strongest
coding agents hit roughly 50–60% on SWE-bench Verified — and those are
curated tasks with clear issues and ready-made tests. Hand the same agent
your everyday work — vague specs, no tests, business rules living in one
teammate's head and a Slack thread from March — and it runs for 20 minutes,
says "all done," and you find it added the feature, broke the tests, and
built the wrong thing anyway.

Most people's first reaction: "the model isn't good enough — let me pay for
a bigger one." Put the wallet away.

## Same horse, different tack

Anthropic ran a controlled experiment: same prompt, same model (Opus 4.5),
two runs. Bare, no support: 20 minutes, $9, core features broken. Full
harness — planner, generator, evaluator: 6 hours, $200, fully working.
**They didn't change the model. They changed the tack.** OpenAI's harness
engineering write-up says it bluntly: a well-harnessed repo takes Codex from
"unreliable" straight to "reliable" — a qualitative leap, not a bit better.
Their million-line experiment (three engineers, zero hand-written code,
1,500 PRs in five months) found every failure came down to one question:
*what is the agent still missing, and can it be supplied in a way that's
understandable and executable?*

**Harness** = everything outside the model weights. If it's not weights,
it's harness. And when things fail: **fix the harness first, then blame the
model.** If the same model succeeds on well-structured tasks, it's a harness
problem.

## Where agents actually get stuck

Five failure modes. Five subsystems. `setup-harness` installs the fix for
each:

| Failure mode | Subsystem | What gets installed |
|---|---|---|
| Vague requirements — the agent guesses | **State** | `features.json`: every feature = behavior + verification command + state. A Definition of Done the agent can run, not vibe |
| Implicit conventions — the agent has never seen the rule | **Instructions** | `AGENTS.md` ≤200 lines: stack, hard constraints (each with teeth), topic docs. The rule leaves your head and enters the repo |
| Broken environment — context burned on `pip install` | **Environment** | One-command setup + a session-start init checklist that *proves* the machine works before work starts |
| No way to run anything | **Tools** | setup / check / dev / logs — non-interactive, stable exit codes, one obvious place |
| "Done when it feels done" — the verification gap | **Feedback** | Three validation layers, kind-shaped: browser evidence for frontend, plan-never-apply for infra, statistical evals for AI agents. Plus a check with teeth |
| Cross-session amnesia — every session re-explores | **State** | `Progress.md` + exact resume steps. Progress lives in files, not chat windows |

Miss any subsystem and everything downstream gets weird. That's the whole
diagnostic loop: execute → observe failure → attribute it to a layer → fix
that layer → never fail that way again. "The model isn't good enough"
should appear less and less in your logs.

One `AGENTS.md` can beat a model upgrade. That is not a joke — it's the
highest-ROI move in harness engineering, and it's this skill's first move.

## One command, three moods

| You point it at | It |
|---|---|
| An empty folder | **Bootstraps** a fresh harness from templates |
| A crusty existing repo | **Adapts**: inventory → morph → archive the confusing stuff |
| An already-harnessed repo | **Reconciles**: drift audit + repair |

Detected from disk — it never asks what it can figure out. Idempotent —
re-running it *is* the health check. And it **archives, never deletes**:
confusing docs move to `archive/` with a manifest row (what, why, when).

## Opinions, held strongly

- **The doer is never the grader.** Builders mark work `built`; only a
  fresh-context, refuting-stance review flips it to `verified`. This repo's
  own features went through that gate.
- **Confidence is not evidence.** Every claim cites a command run this
  session, or it's `not verified`. The agent's "I'm done" is the single most
  common lie in the business.
- **1 UP.** One feature in flight. A PR is a delivery vehicle, not progress.
- **Every doc is accurate or archived.** No third state. Stale docs poison
  every fresh context that reads them.
- **Constraints need teeth.** An architecture rule without a test, lint,
  contract, or rubric is a wish.
- **The acceptance test is a stranger.** Done = a fresh agent survives a
  real task and a newcomer is productive in under 10 minutes.
- **Coordinators write preambles.** When one agent delegates to another,
  run-specific rules (subagent policy, budget, scope) get inserted at the
  top of the delegate's AGENTS.md view — see the template's delegation
  section.

## It enforces itself

```bash
./bin/check   # green, or an actionable reason why not
```

This repo eats its own harness: capped AGENTS.md, schema-validated
features.json, 1 UP enforced, and a check that goes **red** if a standard
loses its teeth, a second feature sneaks into flight, or `skills/` grows a
dependency. Proven red on purpose, three ways, before the first commit.

## Layout

```text
skills/setup-harness/
  SKILL.md                     the whole skill in one screen (≤120 lines, enforced)
  standards/                   the five subsystems: what / why / verify / fix
  kinds/                       backend · frontend · cli · infra · devops · ai-agent
                               — each: real proof surface + safety gates
  references/working-styles.md how change reaches reality, 8 styles
  templates/                   AGENTS.md · Progress.md · features schema ·
                               QUALITY.md · archive manifest
bin/check                      the gate with teeth
principles.md                  the doctrine log this was built from
```

## Use it

Symlink `skills/setup-harness` into your harness's skills directory, then
invoke `setup-harness` in the target repo. Nothing it installs references
this repo — uninstalling is a no-op for the host. Standalone, small,
opinionated.

## Further reading

- OpenAI — *Harness Engineering: Leveraging Codex in an Agent-First World*
- Anthropic — *Effective Harnesses for Long-Running Agents*
- HumanLayer — *Skill Issue: Harness Engineering for Coding Agents*
- SWE-bench Verified leaderboard
- Thoughtworks Technology Radar — *Harness Engineering*

---

*Built for a world where your next teammate might clone the repo — or be
spawned into it.*
