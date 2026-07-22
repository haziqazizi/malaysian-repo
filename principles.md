# Principles

Session notetaker log. Updated live. Principles only land here when stated, decided, or proven — not guessed.

## Session

| Field | Value |
|---|---|
| Date | 2026-07-22 |
| Workspace | `/Users/haziqazizi/code/malaysian-harness` (renamed from `malaysian-engineering-v2`) |
| Mode | notetaker + principles capture |
| Host state | only `principles.md` so far |
| Topic | Agent harness / repo operating system design |

## Working agreements

- Notetaker owns this file. User talks; notes get written here.
- Capture: principles, decisions, constraints, open questions, action items, evidence.
- Prefer short, testable statements over slogans.
- Mark provenance: `user said` / `decided` / `inferred` / `open`.

---

## Principles

### 1. Harness = five subsystems

Status: `user said`

An agent harness is five parts. Name them; design for each.

| Subsystem | Role (captured / inferred) |
|---|---|
| **Instructions** | What the agent is told (AGENTS.md, topic docs, constraints) |
| **Tools** | What the agent can call |
| **Environment** | Where it runs; must be runnable and checked |
| **State** | Durable progress, feature state, session state |
| **Feedback** | Validation, quality signals, verification outcomes |

Missing any one = incomplete harness.

### 2. AGENTS.md is the short front door

Status: `user said`

- **Max 200 lines.** Hard cap.
- Contents:
  1. **Project overview**
  2. **Quick setup**
  3. **Hard constraints**
  4. **Topic docs** pointers (db rules, testing, API design, etc.) — not the full rules inline
  5. **At session start** — checklist / ritual
  6. **At session end** — checklist / ritual

Deep doctrine lives in topic docs. AGENTS.md routes; it does not encyclopedia.

### 3. Initialization check before work

Status: `user said`

Session start proves the machine is real:

- Runnable environment
- Test framework is working
- Explicit **checklist** (not vibes)

No init proof → do not treat the environment as ready.

### 4. Progress.md owns task breakdown

Status: `user said`

- Decompose work so tasks **do not overlap**, **or** run **one at a time**
- **Default to 1 UP** (one unit of progress / one active workstream)
- **1 UP unit (decided 2026-07-22):** one feature from features.json in-flight
  at a time; a PR is just its delivery vehicle
- Progress is durable state, not chat memory

### 5. Feature list is a state machine (JSON)

Status: `user said`

Each feature records:

| Field | Meaning |
|---|---|
| **behavior** | What it does (spec of behavior) |
| **verification command** | How to prove it |
| **state** | Where it is in the machine |

Use **JSON**. Feature list is machine-readable state, not prose wishlist.

### 6. Validation has three layers

Status: `user said`

1. **Syntax / static** — parse, lint, type, contract shape
2. **Runtime** — tests
3. **System-level confirmation** — end-to-end / whole-system proof

Passing only one layer is not "validated."

### 7. Enforce invariants; do not micromanage implementation

Status: `user said`

- Lock the **what must stay true**
- Do **not** dictate every implementation move
- Agents choose path inside the invariant fence

### 8. Architecture constraints need teeth

Status: `user said`

Every arch constraint should have at least one of:

- **Test**
- **Lint rule**
- **Contract**
- **Rubric**

Constraint without enforcement = wish.

### 9. Clean slate

Status: `user said`

Prefer clean slate (exact scope still open — see questions). Capture: start from known-good empty or resettable state rather than muddy inherited context when designing the harness/process.

### 10. Quality document grades modules, not vibes

Status: `user said`

Per-module quality report. Fixed dimensions:

- Verification passing
- Agent understandable
- Test stability
- Architecture boundaries
- Code conventions

Letter grade + structured bullets. Examples (user-provided):

```markdown
## User Authentication Module (Quality: A)
- Verification passing: Yes
- Agent understandable: Yes
- Test stability: Stable
- Architecture boundaries: Compliant
- Code conventions: Followed

## Payment Module (Quality: C)
- Verification passing: Partial (payment callback untested)
- Agent understandable: Difficult (logic spread across 3 files)
- Test stability: Unstable (2 flaky tests)
- Architecture boundaries: Violations present
- Code conventions: Partially followed
```

### 11. Three agent roles

Status: `user said`

| Role | Job (minimal capture) |
|---|---|
| **Explorer** | Discover, map, reduce uncertainty |
| **Implementor** | Build against invariants / feature state |
| **Verifier** | Run validation layers; grade quality |

Separate discovery, build, and proof. Do not collapse all three into one unaccountable pass.

### 12. Working styles — how change reaches reality

Status: `user said` (the two) + `inferred` (taxonomy)

User: two ways of working — **deploy to prod** vs **work in prod**. Others
observed across industry:

| Style | Change travels by | Rollback |
|---|---|---|
| Deploy to prod (pipeline) | local → CI → artifact → deploy | redeploy previous |
| Work in prod (dev = prod) | edit live thing, restart | backup-before-touch |
| Trunk + flags | merge fast, ship dark, ramp % | flag flip |
| Release trains / versioned artifacts | cut version, users install | ship another version |
| On-prem / client-installed | installer + release notes | customer-side |
| GitOps / declarative | edit desired state; reconciler applies | revert the declaration |
| Batch / data | update job in DAG | backfill |
| Regulated / change-managed | change board, signed artifacts, audit trail | formal event |

Axes: where you edit (local copy vs live thing) × how change travels (gates vs
direct) × substrate reversibility. Harness surfaces (deploy ladder vs
backup-before-touch) must match the repo's style — wrong surface = unused ritual.

### 13. malaysian-harness = the setup-harness skill

Status: `user said` (direction) + `inferred` (mechanics)

Goal: a skill that sets repos up for success — a joy for both humans and AI
agents to work in. Single command `setup-harness`:

- **Bootstrap** — empty folder → scaffold from templates
- **Adapt** — existing repo → inventory → map → morph
- **Reconcile** — already-harnessed → re-run = drift audit + repair

Same command, idempotent; re-run is the health check.

Mechanics (inferred, open to revision):

- Checklist organized by the 5 subsystems (principle 1), not a flat list.
- **Accurate or archived** — no third state for docs. `archive/` + manifest
  (what, why, date). Archive, never delete.
- Detect working style (principle 12) before building surfaces.
- Acceptance test = fresh-agent soak (doer ≠ grader) + human day-one test
  (clone → running → tested → knows next task, <10 min).
- Run discipline from me-setup-repo: inventory before building; live evidence
  only; gaps fix-now / report-only / skip; skips are recorded decisions;
  owner-only decisions (deletions, deploy, secrets) block.
- **Standalone, small, opinionated** (decided): reuses ME's ideas, no symlink
  or runtime dependency on `malaysian-engineering`.

### 14. Monorepo: harness is fractal

Status: `user said`

If the repo is a monorepo, the relevant subfolders must also carry the
relevant files — harness surfaces exist at every level that agents work at,
not only the root. Root routes; each relevant subfolder (package/app/service)
gets its own applicable surfaces.

File matrix (decided 2026-07-22):

| Surface | Root | Each relevant subfolder |
|---|---|---|
| AGENTS.md | yes — routes to subfolders | yes — always (own or pointer) |
| Check command | yes — whole-repo gate | yes — always (package-scoped) |
| features.json | yes | only if independently shipped |
| Progress.md | yes | only if independently shipped |

Rationale: per-package state everywhere fragments state — 1 UP silently
becomes N UP. Per-package state is earned by independent shipping, not by
folder existence.

### 15. Verification and safety are kind-shaped

Status: `user said` (the question/direction) + `inferred` (matrix)

Different project kinds need different proof surfaces and different safety
gates. setup-harness detects kind(s) — per package in a monorepo — and
installs kind-specific surfaces. One generic checklist misses the point.

| Kind | Proof surface | Danger zone |
|---|---|---|
| Backend | tests + request/response + logs + DB state | migrations/data are one-way doors; deploy ≠ migrate; auth/tenancy; API backcompat |
| Frontend | browser evidence: screenshots, e2e, visual diff (static passing means little) | secrets in client bundle, XSS, PII in analytics |
| CLI/library | deterministic: golden files, exit codes (highest test leverage) | destructive file ops → dry-run first; flags are the API; backcompat |
| Infra/IaC | `plan` diff + post-apply smoke (plan ≠ apply; drift) | biggest blast radius: stateful resource replace/delete, IAM widening, cost; rollback often impossible → plan-never-apply, read-only creds by construction |
| DevOps/CI | pipeline only truly runs in pipeline env; branch dry-runs | meta-risk: CI change modifies the gate itself → protect the verifier; secrets in logs; unpinned actions |
| AI agent | statistical: evals + baselines + regression sets + LLM-judge tier + blind runs | prompt injection (loaded content is data), over-broad tool perms, runaway cost loops, exfil via tools, gaming own graders |

Consequences:

- The 3 validation layers (principle 6) are kind-shaped: "system-level
  confirmation" = browser evidence (frontend), plan+smoke (infra), eval suite
  (agents), etc.
- features.json `verification command` must match the kind's real proof
  surface, not just "tests pass".
- Safety gates installed per kind: migration approval (backend), dry-run
  flags (CLI), plan-only creds (infra), protected CI config (devops), budget
  caps + un-gameable graders (agents).

### 16. Core thesis: strong models ≠ reliable execution — fix the harness first

Status: `user said`

The README's center of gravity. Reliable execution toward a task, by humans
or AIs, comes from the harness, not the model. Evidence (user-supplied
lecture): SWE-bench ~50–60% on curated tasks, worse on real ones; Anthropic
same-model experiment (bare: $9/20min/broken vs harnessed: $200/6h/working);
OpenAI: well-harnessed repo takes Codex "unreliable" → "reliable";
million-line experiment (3 engineers, 0 hand-written lines, 1,500 PRs/5mo).

Rules: when things fail, check the harness before blaming the model.
Attribute every failure to a layer (task spec, context, environment,
verification, session state — maps onto the 5 subsystems). Diagnostic loop:
execute → observe → attribute → fix layer → never fail that way again.
Definition of Done = conditions verifiable by command. One AGENTS.md can
beat a model upgrade.

Key terms adopted: capability gap, harness, harness-induced failure,
verification gap, diagnostic loop, definition of done, context anxiety.

### 17. AGENTS.md carries a runtime delegation preamble

Status: `user said`

- AGENTS.md (the template) has a PREAMBLE SLOT at top: empty when committed;
  a coordinating agent inserts run-specific instructions at runtime when
  delegating; the delegate reads it first.
- Bottom section addresses coordinators: when delegating, insert a preamble
  with anything the delegate must know — e.g. "don't spawn subagents",
  "use a workflow", "decompose recursively into the smallest unit", budget,
  scope, forbidden actions, report-back format.
- Preambles narrow authority/scope, never widen; hard constraints win over
  preambles; preambles are never committed.

### 18. Session end leaves the toolbelt consolidated, fast, narrow-and-deep

Status: `user said`

At session end, the agent makes sure the setup/feedback scripts are
**consolidated** (not too many — minimize cognitive complexity for the next
agent) and **fast**. Complexity is nested in **narrow and deep**
modules/scripts: small, simple entry points; depth hidden inside.

- New capability folds into an existing entry point (flag/subcommand), not a
  new top-level script. A fifth top-level command needs a recorded reason.
- Slow checks are debt: the next agent runs them every session start.
- The measure is the next agent's cognitive load, not this agent's
  convenience.

---

## Artifact map (inferred from dump)

Status: `inferred` — filenames/shape stated or strongly implied

| Artifact | Purpose |
|---|---|
| `AGENTS.md` | ≤200 lines: overview, setup, hard constraints, topic pointers, session start/end |
| Topic docs | Deep rules (db, testing, API design, …) |
| Init checklist | Runnable env + test framework working |
| `Progress.md` | Task breakdown; non-overlap; default 1 UP |
| Feature list (JSON) | behavior + verification command + state (state machine) |
| Quality doc | Per-module grade + five dimensions |
| Arch enforcement | test / lint / contract / rubric per constraint |

---

## Decisions

| When | Decision | Why | Status |
|---|---|---|---|
| 2026-07-22 | Use `principles.md` as session source of truth | User requested | decided |
| 2026-07-22 | Harness framed as 5 subsystems | User stated | decided |
| 2026-07-22 | AGENTS.md hard-capped at 200 lines | User stated | decided |
| 2026-07-22 | Default concurrency = 1 UP | User stated | decided |
| 2026-07-22 | Feature list = JSON state machine | User stated | decided |
| 2026-07-22 | Validation = static + runtime + system | User stated | decided |
| 2026-07-22 | Roles = explorer / implementor / verifier | User stated | decided |
| 2026-07-22 | Folder renamed `malaysian-engineering-v2` → `malaysian-harness` | User requested | decided |
| 2026-07-22 | Two base working styles: deploy-to-prod vs work-in-prod | User stated | decided |
| 2026-07-22 | malaysian-harness = `setup-harness` skill: bootstrap / adapt / reconcile, one command | User stated | decided |
| 2026-07-22 | Old/confusing content gets archived, not deleted | User stated | decided |
| 2026-07-22 | setup-harness is standalone, small, opinionated — no dependency on ME | User stated | decided |
| 2026-07-22 | Monorepos: relevant subfolders must carry the relevant harness files | User stated | decided |
| 2026-07-22 | Monorepo matrix: AGENTS.md + check always per package; features.json/Progress.md only if independently shipped | User accepted rec | decided |
| 2026-07-22 | 1 UP = one feature from features.json in-flight; PR is the delivery vehicle | User accepted rec | decided |
| 2026-07-22 | Verification + safety gates are per-project-kind (backend/frontend/CLI/infra/devops/AI-agent), detected per package | User directed; matrix inferred | decided |
| 2026-07-22 | README centers the harness-first thesis: strong models ≠ reliable execution | User stated | decided |
| 2026-07-22 | AGENTS.md template gets a runtime PREAMBLE SLOT + coordinator delegation section (narrow-never-widen) | User stated | decided |
| 2026-07-22 | Session end includes toolbelt consolidation: few + fast entry points; complexity in narrow-and-deep scripts | User stated | decided |

## Constraints & non-goals

**Constraints (`user said`)**

- AGENTS.md ≤ 200 lines
- Default one active unit of progress (1 UP)
- Tasks non-overlapping **or** strictly sequential
- Invariants enforced; implementation not micromanaged
- Arch rules need test/lint/contract/rubric
- Feature records: behavior, verification command, state
- Quality grades use the five fixed dimensions

**Non-goals**

_None stated yet._

## Open questions

1. **Clean slate** — clean slate of what? empty repo? reset progress? wipe agent context each session?
2. ~~1 UP~~ — resolved: one feature from features.json in-flight; PR is the delivery vehicle.
3. **Feature list path** — e.g. `features.json` / `docs/features.json`?
4. **Quality doc path** — e.g. `QUALITY.md`?
5. **Progress.md vs feature JSON** — which owns state of work vs product state?
6. **Who runs which role** — same agent sequential, or separate subagents?
7. **Session start/end** — exact checklist items beyond init check?
8. ~~Relationship to ME~~ — resolved: standalone, small, opinionated.
9. **Archive location** — `archive/` vs `docs/archive/`? Manifest format?
10. **Working-style detection** — detect-from-disk signals list; when to ask vs assume?
11. ~~Monorepo file matrix~~ — resolved: see principle 14 matrix.

## Action items

- [x] Capture harness doctrine dump into `principles.md`
- [ ] Resolve open questions (especially clean slate, 1 UP unit, artifact paths)
- [ ] When building: scaffold AGENTS.md + Progress.md + features JSON + quality template + init checklist

## Raw notes

### 2026-07-22 — session open

- User: make `principles.md` and act as notetaker this session.
- Created living log.

### 2026-07-22 — rename workspace

- User: rename the folder you're in to `malaysian-harness`
- Done: `/Users/haziqazizi/code/malaysian-engineering-v2` → `/Users/haziqazizi/code/malaysian-harness`
- Note: session tools may still cache old workspace path until reopened

### 2026-07-22 — harness doctrine dump

User dump (normalized spelling in principles above):

- Harness = 5 subsystems: instructions, tools, environment, state, feedback
- AGENTS.md max 200 lines: proj overview, quick setup, hard constraints, topic docs (db rules, testing, api design, etc.), "At session start", "At session end"
- Initialization check: runnable environment; test framework working; checklist
- Progress.md: task breakdown; decompose so no overlap; or one at a time; default to 1 UP
- Feature list: behavior, verification command, state; state machine; use JSON
- Validation: syntax and static; runtime (tests); system-level confirmation
- Enforce invariants; don't micromanage implementation
- Arch constraints should have a test or lint rule; contract; rubric
- Clean slate
- Quality document format (A/C module examples with 5 dimensions)
- Roles: explorer, implementor, verifier

### 2026-07-22 — working styles + setup-harness direction

- Reviewed `malaysian-engineering` repo: gated skill graph, 13 repo standards,
  doer-never-grader, live-evidence discipline, symlink-only host coupling.
- User: two ways of working — deploy to prod vs work in prod; asked for other
  styles; taxonomy captured as principle 12.
- User: malaysian-harness should be a skill that sets repos up for success —
  joyful for humans and agents. Single `setup-harness` command: start from
  scratch or adapt existing, archive old/confusing content. Captured as
  principle 13.
- User: standalone, small, opinionated — confirmed. Monorepo rule: relevant
  subfolders must also have the relevant files (principle 14).
- User accepted recs: monorepo file matrix (principle 14) and 1 UP = one
  feature in-flight (principle 4).
- User: consider verifiability + safety per project kind (infra, devops,
  frontend, backend, CLI, AI agent). Matrix captured as principle 15.
- User reframed README simply: a good repo = agent can **orient** (map of
  the world), **act** (straightforward tools/scripts + runbooks/skills),
  **get feedback** on results, and **resume** where others left off
  (willingly or unwillingly) — via a couple of mechanisms. README rewritten
  around orient/act/feedback/resume with a mechanisms table.
