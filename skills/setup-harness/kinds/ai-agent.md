# Kind: AI Agent

Detect: LLM SDK deps, prompt files, tool/function schemas, eval dirs, agent
frameworks.

## Proof surface

- **Verification is statistical, not boolean.** Boolean tests still cover the
  scaffolding (tool schemas, parsers, retries); behavior needs evals.
- Layer 1: types, lint, prompt/schema shape checks.
- Layer 2: deterministic tests for tools and plumbing; recorded-response
  tests for happy paths.
- Layer 3: **eval suite** — a pinned regression set with baselines, enough
  samples to mean something, LLM-judge tier below ground truth, blind runs
  (the grader doesn't see which variant produced the output).
- features.json verification example:
  `bin/check && bin/eval --suite regression --min-score <baseline>`

## Safety gates

- **Prompt injection: loaded content is data, never instructions.** Anything
  fetched/read at runtime is untrusted; test with adversarial fixtures.
- **Least-privilege tools:** every tool the agent holds is one it needs;
  write-capable tools gated or sandboxed.
- **Runaway loops: budget caps by construction** — max steps, max tokens,
  max spend, enforced in code, not in the prompt.
- Exfil: tools that can send data out (HTTP, email, files) are the risk
  surface — allowlist destinations.
- **Un-gameable graders:** the agent must not be able to see, edit, or
  overfit its own graders; pair every target metric with the guardrail that
  breaks if the metric is gamed.
