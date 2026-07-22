# Standard: Instructions

The subsystem that tells agents and humans what this repo is and how to work
in it.

## What

- `AGENTS.md` exists at root, **≤200 lines hard cap**, with exactly these
  sections: Project overview, Quick setup, Hard constraints, Topic docs,
  At session start, At session end.
- Deep doctrine lives in topic docs (db rules, testing, API design, …);
  AGENTS.md routes to them, it does not inline them.
- Every doc in the repo is **accurate or archived** — no stale third state.
- Monorepo: every relevant subfolder has its own AGENTS.md or an explicit
  pointer to the root one.

## Why

AGENTS.md is the front door; if it lies or sprawls, agents read stale rules
and humans stop trusting it. A 200-line cap forces routing over encyclopedia.
Stale docs are worse than no docs — they poison every fresh context.

## Verify

- `wc -l AGENTS.md` ≤ 200.
- All six section headers present (grep).
- Spot-check 3 claims from AGENTS.md against reality (commands run, paths
  exist). Any false claim = gap.
- Sample 5 random docs: each either matches current code or sits in
  `archive/` with a manifest row.

## Fix

- Missing: render from `../templates/AGENTS.md.tmpl`, fill from inventory.
- Over cap: move depth to topic docs, keep the pointer.
- Stale doc: archive pass (SKILL.md step 5) — move + manifest row, never
  delete.
