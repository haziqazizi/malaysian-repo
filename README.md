# malaysian-harness

**Reliable execution toward a task — by humans or AIs.**

A good repo is one where an agent can immediately **orient** itself with a
map of the world, pick up straightforward **tools** (scripts) and
**runbooks** (skills) to start work, and get **feedback** on the results of
its actions. A good repo also helps the next agent **resume** where the last
one left off — willingly or unwillingly.

Most repos are not that. The docs lie, setup is archaeology, "done" means
"compiled", and every new session starts from zero. Then the agent fails and
everyone blames the model. It's rarely the model: same model, bare repo →
broken output; same model, well-harnessed repo → working software (Anthropic
and OpenAI have both shown exactly this). **Fix the harness first.**

`setup-harness` is one command that installs the harness.

## The mechanisms

| The agent needs to… | Mechanism | What it is |
|---|---|---|
| **Orient** | `AGENTS.md` (≤200 lines) | The map of the world: what this is, the hard rules (each enforced), where deeper docs live |
| | Session-start checklist | In AGENTS.md: prove the machine is real before working — check green, read Progress.md, confirm the one active task. No init proof, no work |
| | `archive/` + manifest | Every doc is accurate or archived — no stale docs poisoning fresh context |
| **Act** | `bin/` scripts | setup / check / dev / logs — non-interactive, stable exit codes, one obvious place |
| | Runbooks (skills) | Repeatable multi-step work written down, not rediscovered |
| **Get feedback** | `bin/check` | The gate: green, or an actionable reason why not. Goes red when the repo itself drifts |
| | Three validation layers | Static → tests → system-level proof, shaped to what the project *is*: browser evidence for frontend, plan-never-apply for infra, evals for AI agents |
| | `QUALITY.md` | Per-module letter grades from evidence, not vibes |
| **Resume** | Session-end checklist | In AGENTS.md: leave it resumable — check green or failure recorded, state files updated, decisions logged, work committed |
| | `features.json` | Machine-readable feature state: behavior + verification command + state. A Definition of Done the next agent can run |
| | `Progress.md` | One task in flight (1 UP), an ordered queue, and the exact resume step if a session dies mid-work |
| | Preamble slot | A coordinator delegating work inserts run-specific rules (subagent policy, budget, scope) at the top of the delegate's AGENTS.md view |

## One command, three moods

| You point it at | It |
|---|---|
| An empty folder | **Bootstraps** a fresh harness from templates |
| An existing repo | **Adapts**: inventory → morph → archive the confusing stuff |
| A harnessed repo | **Reconciles**: drift audit + repair |

Detected from disk — it never asks what it can figure out. Idempotent —
re-running it *is* the health check. It **archives, never deletes**.

## The opinions underneath

- **The doer is never the grader.** Builders mark work `built`; only a
  fresh-context review flips it to `verified`.
- **Confidence is not evidence.** Every claim cites a command run this
  session, or it's `not verified`.
- **Constraints need teeth.** A rule without a test, lint, contract, or
  rubric is a wish.
- **The acceptance test is a stranger.** Done = a fresh agent survives a
  real task, and a newcomer is productive in under 10 minutes.

This repo eats its own harness: run `./bin/check` — it was proven red on
purpose, three ways, before the first commit.

## Layout

```text
skills/setup-harness/
  SKILL.md                     the whole skill in one screen (≤120 lines, enforced)
  standards/                   five subsystems: what / why / verify / fix
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
this repo — uninstall is a no-op for the host. Standalone, small,
opinionated.

## Further reading

- OpenAI — *Harness Engineering: Leveraging Codex in an Agent-First World*
- Anthropic — *Effective Harnesses for Long-Running Agents*
- HumanLayer — *Skill Issue: Harness Engineering for Coding Agents*

---

*Built for a world where your next teammate might clone the repo — or be
spawned into it.*
