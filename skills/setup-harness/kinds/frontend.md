# Kind: Frontend

Detect: package.json with UI frameworks, component dirs, bundler configs,
public/static assets.

## Proof surface

- Layer 1: types, lint. Weakest signal of any kind — passing means little.
- Layer 2: component/unit tests.
- Layer 3 (the real one): **browser evidence** — screenshots, interaction
  e2e, visual diff against a baseline. "Looks right" needs eyes or pixels,
  not just green tests. Accessibility checks ride here.
- features.json verification example:
  `bin/check && npx playwright test <flow>.spec.ts` (or screenshot + visual
  diff for pure-visual features).

## Safety gates

- **Nothing secret enters the client bundle.** Grep build output for known
  secret patterns / server-only env vars as part of `check`.
- XSS: any HTML-from-data path gets a sanitization test.
- PII does not enter analytics events without a recorded owner decision.
- Blast radius is low but verification is the hard part — budget the e2e
  layer first, not last; a flaky e2e suite is a gap, not a skip.
