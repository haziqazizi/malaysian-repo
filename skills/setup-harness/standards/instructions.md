# Standard: Instructions

The subsystem that tells agents and humans what this repo is and how to work
in it.

## What

- `AGENTS.md` exists at root, **≤200 lines hard cap**, with exactly these
  sections: Project overview, Quick setup, Hard constraints, Topic docs,
  At session start, At session end. The root entry file stays a **router of
  50–200 lines**: it points, it does not explain.
- Deep doctrine lives in topic docs (db rules, testing, API design, …);
  AGENTS.md routes to them, it does not inline them.
- **Closed-set docs structure.** `docs/` holds exactly these slots:

  | Slot | Contents |
  |---|---|
  | Noun routers | `architecture/` `development/` `testing/` `releases/` `sessions/` |
  | Machine dirs | `generated/` `references/` |
  | Data files | `features.json`, the papercuts / friction log |

  Every doc is a router `README.md` or is listed in its router's README. A
  new router needs owner approval. why: a closed set turns "where does this
  go?" into a lookup. source: monorepo field deployment, 2026-08.
- **Routing limits**: at most 5 choices per hop, at most 3 hops from the root
  entry file to any answer. why: an agent that reads more than 3 files to
  find a rule stops reading. source: monorepo field deployment, 2026-08.
- **Two-interface waterline.** The repo carries a human interface (prose,
  rationale, history) and an agent interface: **3 reads, a handful of verbs,
  obey the failing check**. Rules reach agents through checks, not through
  remembered prose. A rule MAY live as prose only when no check can own it,
  and that exemption is written next to the rule. why: prose degrades with
  context length; a failing check does not. source: monorepo field
  deployment, 2026-08.
- **Provider and tool modularity.** Provider specifics (cloud, CI, payments,
  model vendor) live behind one adapter in code plus one leaf doc. Routers
  and rule docs stay provider-free. why: a provider swap must touch two
  files, not the doc tree. source: monorepo field deployment, 2026-08.
- **Rules carry provenance.** Every MUST / MUST NOT line ends with a short
  `why:` or `source:` note. why: a rule with no reason is deleted by the
  next agent, or obeyed after it stops being true.
- Every doc in the repo is **accurate or archived** — no stale third state.
- Monorepo: every relevant subfolder has its own AGENTS.md or an explicit
  pointer to the root one.

## Why

AGENTS.md is the front door; if it lies or sprawls, agents read stale rules
and humans stop trusting it. A 200-line cap forces routing over encyclopedia.
Stale docs are worse than no docs — they poison every fresh context. An open
docs structure grows one file per opinion; a closed set forces every opinion
into an owned slot or out of the repo.

## Verify

- `wc -l AGENTS.md` is 50–200.
- All six section headers present (grep).
- Spot-check 3 claims from AGENTS.md against reality (commands run, paths
  exist). Any false claim = gap.
- **Unrouted-file probe.** List every file under `docs/`. Each is a router
  README, an entry in its router's README, a machine dir, or a data file.
  The check refuses unrouted files and names the outlets: add a router
  entry, move to `archive/`, or file a papercut.
- Count choices per hop (≤5) and hops to an answer (≤3) for three real
  questions: how do I run it, how do I verify, what ships.
- Sample 5 MUST / MUST NOT lines: each carries a `why:` or `source:` note.
- Grep routers and rule docs for provider names: a hit outside the adapter's
  leaf doc is a gap.
- Sample 5 random docs: each matches current code or sits in `archive/` with
  a manifest row.

## Fix

- Missing: render from `../templates/AGENTS.md.tmpl`, fill from inventory.
- Over cap: move depth to topic docs, keep the pointer.
- Unrouted doc: route it, archive it, or convert it to a papercut. Never
  leave a fourth state.
- Rule with no check: write the check, or write the exemption beside the
  rule. Prose alone is the exception, never the default.
- Provider name in a router: move the detail behind the adapter's leaf doc
  and leave a pointer.
- New router requested: stop and ask the owner.
- Stale doc: archive pass (SKILL.md step 5) — move + manifest row, never
  delete.
