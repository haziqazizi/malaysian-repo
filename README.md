# malaysian-harness

**Most repos are hostile territory.** Setup takes an afternoon of archaeology.
The docs lie. Tests pass locally and fail in CI. Three TODO files contradict
each other. A fresh AI agent walks in, reads a stale README, and confidently
breaks production.

`setup-harness` is one command that turns that repo into a place where humans
and AI agents actually *want* to work.

## The idea

A repo is a harness for whoever works in it — and a harness has exactly
**five subsystems**. Miss one and everything downstream gets weird:

| Subsystem | The question it answers | Without it |
|---|---|---|
| **Instructions** | What is this? What are the rules? | Agents obey docs that stopped being true in March |
| **Tools** | What can I run? | Every session starts with archaeology |
| **Environment** | Does this machine even work? | "Works on my machine" forever |
| **State** | What's in flight? What's next? | Progress dies with the chat window |
| **Feedback** | Was that actually good? | "Done" means "compiled" |

`setup-harness` installs all five — shaped to what the project *is* (backend,
frontend, CLI, infra, devops, AI agent) and how it *ships* (deploy-to-prod,
work-in-prod, trunk-and-flags, release trains…). A frontend gets browser
evidence, not vibes. Infra gets plan-never-apply, not YOLO. An AI agent
project gets statistical evals and budget caps, not a prayer.

## One command, three moods

| You point it at | It | 
|---|---|
| An empty folder | **Bootstraps** a fresh harness from templates |
| A crusty existing repo | **Adapts**: inventory → morph → archive the confusing stuff |
| An already-harnessed repo | **Reconciles**: drift audit + repair |

Detected from disk — it never asks what it can figure out. Idempotent —
re-running it *is* the health check. And it **archives, never deletes**:
every confusing doc moves to `archive/` with a manifest row saying what,
why, when. Reversible by design.

## Opinions, held strongly

- **The doer is never the grader.** Builders mark work `built`. Only a
  fresh-context review — different agent, refuting stance — flips it to
  `verified`. This repo's own features went through exactly that gate.
- **Confidence is not evidence.** Every claim cites a command run *this
  session*, or it's `not verified`. Docs saying so counts for nothing.
- **1 UP.** One feature in flight at a time. A PR is a delivery vehicle,
  not a unit of progress.
- **Every doc is accurate or archived.** There is no third state. Stale
  docs don't just rot — they poison every fresh context that reads them.
- **Constraints need teeth.** An architecture rule without a test, lint,
  contract, or rubric is a wish.
- **The acceptance test is a stranger.** Setup isn't done when the setup
  agent says so — it's done when a *fresh* agent survives a real task and a
  newcomer is productive in under 10 minutes.

## It enforces itself

```bash
./bin/check   # green, or an actionable reason why not
```

This repo eats its own harness: `AGENTS.md` (capped at 200 lines — enforced),
`features.json` (schema-validated, 1 UP — enforced), `Progress.md`, and a
check that goes **red** if a standard file loses its teeth, a second feature
sneaks into flight, or anything under `skills/` grows a dependency. Proven
red on purpose, three ways, before the first commit.

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

## Develop it

```bash
./bin/check   # keep it green
```

Doctrine changes start life in `principles.md`, then land in the skill.
Several disciplines here are sharpened versions of ideas from the author's
ME system — deliberately reimplemented, zero dependency.

---

*Built for a world where your next teammate might clone the repo — or be
spawned into it.*
