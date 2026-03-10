# Report Template — Fact Check

Generate the final report following exactly this structure:

```markdown
# Fact Check

**Claim:** [claim or article title]
**Source:** [media/author] — Reliability: [Tier X]
**Date:** [verification date]
**Overall Verdict:** [per VERDICT_SCALE.md]
**Confidence:** [HIGH / MEDIUM / LOW]

---

## Summary

[2-3 sentences: what is true, what is exaggerated/false, and why]

## Decomposition of Claims

| # | Sub-claim | Verdict | Confidence | Sources |
|---|-----------|---------|------------|---------|
| 1 | [specific fact] | [verdict] | [level] | [refs] |
| 2 | [cited figure] | [verdict] | [level] | [refs] |
| 3 | [attribution/citation] | [verdict] | [level] | [refs] |

## Missing Context

[Important elements omitted by the original article that change perception]

## Detected Biases

| Bias | Detail |
|------|--------|
| [bias type] | [concrete explanation] |

## What Primary Sources Actually Say

| Primary Source | What It Actually Says | Gap with Article |
|----------------|----------------------|------------------|
| [study/report/statement] | [citation or summary] | [possible distortion] |

## Official Positions (if applicable)

| Organization | Position | Date |
|--------------|----------|------|
| [ex: CNRS] | [position] | [date] |

## Sources Consulted

| # | Source | Type | Reliability | URL |
|---|--------|------|-------------|-----|
| 1 | [name] | [type] | [Tier X] | [url] |
| 2 | [name] | [type] | [Tier X] | [url] |
```

## Template Rules

- Each sub-claim gets its own independent verdict
- Always include missing context, even if it doesn't invalidate the article
- Biases are factual, not value judgments
- Cite primary sources directly, not reprints
- Do not leave cells empty — write "Not found" if information is missing
- The overall verdict is the weighted synthesis of sub-verdicts
- Explicitly flag headline/content gaps if present
