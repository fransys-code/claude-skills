# Report template — Science Check

Generate the final report by following this structure exactly:

```markdown
# Scientific verification

**Claim:** [original claim]
**Date:** [date]
**Overall verdict:** [CONFIRMED / PARTIALLY CONFIRMED / NOT PROVEN / REFUTED / PREMATURE]

---

## Scientific consensus

[Summary in 2-3 sentences of what current science says]

## Detail by sub-claim

| Specific claim | Verdict | Evidence level | Key sources |
|---|---|---|---|
| [ex: reduces blood pressure] | Confirmed | Meta-analysis RCT (n=2400) | [ref] |
| [ex: boosts energy] | Not proven | 1 negative RCT (n=45) | [ref] |

## Risks and side effects

| Risk | Severity | Evidence level | Population affected |
|---|---|---|---|
| [risk] | [mild/moderate/severe] | [study type] | [who is affected] |

## Official positions

| Organization | Position | Date |
|---|---|---|
| FDA | [position] | [date] |
| EFSA | [position] | [date] |
| WHO | [position] | [date] |
| ANSES | [position] | [date] |

## Red flags identified

- [Industry funding]
- [Studies not replicated]
- [Sample sizes too small]
- [Retractions]
- [Conflicts of interest]

## Sources consulted

| # | Source | Type | Year | URL |
|---|---|---|---|---|
| 1 | [name] | Meta-analysis | [year] | [url] |
| 2 | [name] | RCT | [year] | [url] |
| 3 | [name] | Regulatory position | [year] | [url] |
```

## Template rules

- Each sub-claim must have its own independent verdict
- Always include sample size (n=) when available
- Do not leave cells empty — write "Not found" if information is missing
- The overall verdict is the weighted synthesis of sub-verdicts (one refuted sub-claim can result in "PARTIALLY CONFIRMED" if others are confirmed)
- Sources are numbered and referenced in the sub-claims table
