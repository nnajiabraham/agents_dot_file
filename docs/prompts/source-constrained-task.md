# Prompt Title: Source Constrained Task

## Prompt Template

```md
Pretend you know nothing about [DOMAIN] except:
[ALLOWED KNOWLEDGE]

Read [URL or @path/to/doc] as the only allowed source.

Goal:
[TASK]

Deliver:
1. A sufficiency verdict: either "sufficient" or "insufficient".
2. If sufficient, the exact steps needed to complete the goal using only this source.
3. If insufficient, stop at the first unstated assumption, missing prerequisite, or unclear instruction and name it exactly.

Rules:
- Derive every step only from what the source explicitly states.
- Do not use outside knowledge beyond the stated baseline.
- Do not use other docs, examples, conventions, or training data.
- Do not guess or fill in missing steps.
```
