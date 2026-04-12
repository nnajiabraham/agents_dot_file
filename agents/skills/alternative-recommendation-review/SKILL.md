---
name: alternative-recommendation-review
description: Compare a proposed alternative approach to the agent's prior recommendation with emphasis on integration effort and security. Use when the user pastes a counter-proposal and wants an explicit agree/disagree and updated stance.
---

# Alternative recommendation review

The user supplies **alternative research or a recommendation**. Compare it to **your current recommendation** (summarize your prior stance if the chat does not already contain it).

## When to use

- User pastes third-party advice, another LLM output, or a competing design and asks how it compares.

## Inputs to collect

- **Your recommendation** (bullet summary: approach, key tech, tradeoffs).
- **Alternative** (full paste or linked content).
- **Constraints** the user cares about (time, team skills, compliance, etc.) if not already known.

## Workflow

1. Restate both options in one paragraph each, neutrally.
2. Compare on:
   - **Ease of integration** with the existing codebase, data model, and ops (concrete: APIs, migrations, deployment, ownership).
   - **Security** (authn/z, secrets, supply chain, data exposure, threat model fit).
   - **Operational cost** (complexity, monitoring, failure modes) if relevant.
3. State clearly: **agree**, **disagree**, or **partially agree** with the alternative as the better path.
4. Explain **how your stance changes**, if at all: what you would adopt, reject, or hybridize.
5. If evidence is missing (e.g. alternative omits versioning or threat analysis), say what would be needed to decide.

## Output format

- **Verdict:** [agree / disagree / partial] with the alternative vs your prior recommendation
- **Integration analysis:** [short]
- **Security analysis:** [short]
- **Updated recommendation:** [what you would do now]

## Example

Prior: use managed Postgres + migrations in-repo. Alternative: embed SQLite per tenant. Verdict: partial — faster MVP locally but weak multi-tenant backup/isolation story; would only agree for a strictly single-tenant tool with explicit export path.
