---
name: science-check
description: "Verifies a scientific or health claim by cross-checking PubMed, Semantic Scholar, and the web. Produces a structured report with evidence levels, risks, and official positions. Use this skill whenever the user asks a question about health, nutrition, dietary supplements, medications, therapies, or asks if a scientific claim is true, proven, or reliable, even if the question is informal."
user-invocable: true
argument-hint: "[claim to verify]"
allowed-tools:
  - Agent
  - Bash
  - Read
  - WebSearch
  - WebFetch
  - AskUserQuestion
  - Write
  - mcp__pubmed__search_pubmed
  - mcp__pubmed__get_paper_fulltext
  - mcp__paper-search__search_pubmed
  - mcp__paper-search__search_arxiv
  - mcp__paper-search__search_google_scholar
  - mcp__paper-search__search_biorxiv
  - mcp__paper-search__search_medrxiv
  - mcp__paper-search__read_pubmed_paper
  - mcp__paper-search__read_biorxiv_paper
  - mcp__paper-search__read_medrxiv_paper
---

# Science Check

Verifies what science says about a given claim by cross-referencing multiple sources and producing a structured report.

## Progression

Copy and follow this checklist:

```
- [ ] Claim retrieved and translated to English
- [ ] ORIENTATION phase: 3 parallel searches launched
- [ ] DEEPENING phase: reference sources fetched
- [ ] VALIDATION phase: key studies verified
- [ ] SELF-VERIFICATION phase: quality checklist validated
- [ ] SYNTHESIS phase: report generated
```

## 1. Retrieve the claim

- If `$ARGUMENTS` is provided, use it as the claim to verify.
- Otherwise, ask the user via `AskUserQuestion`.
- Formulate the search query in English.

## 2. ORIENTATION phase — 3 parallel searches

Launch the 3 searches **in parallel via the Agent tool** (one per sub-agent):

**Agent A — Meta-analyses and systematic reviews**
- WebSearch: `"[subject] systematic review meta-analysis site:pubmed.ncbi.nlm.nih.gov OR site:cochranelibrary.com"`
- If MCP pubmed/paper-search available: search with "meta-analysis" or "systematic review" filter

**Agent B — Risks and negative effects**
- WebSearch: `"[subject] risks side effects adverse scientific evidence"`

**Agent C — Critical position / debunking**
- WebSearch: `"[subject] claims debunked OR confirmed OR evidence-based science"`

## 3. DEEPENING phase — Reference sources

Fetch the best sources found, prioritizing reliable sources listed in [TRUSTED_SOURCES.md](TRUSTED_SOURCES.md).

If MCP paper-search available: search on Semantic Scholar / Google Scholar for citations and impact.

## 4. VALIDATION phase — Cross-verification

For the 3-5 key studies identified:
- Sample size (n=?)
- Study type (RCT, observational, animal, in vitro)
- Funding (industry = potential bias)
- Replication of results
- Verify via WebSearch if retracted (Retraction Watch)

## 5. SELF-VERIFICATION phase — Quality checklist

Before writing the report, verify:
- [ ] At least 3 independent sources consulted
- [ ] At least 1 meta-analysis or systematic review found (otherwise report it)
- [ ] No conclusion based on a single study
- [ ] Risks and side effects identified (even if the claim is only about benefits)
- [ ] If no solid evidence found → verdict = "PREMATURE" or "NOT PROVEN"

If a point fails, launch targeted searches before moving to synthesis.

## 6. SYNTHESIS phase — deep thinking

Analyze all evidence with thorough reasoning, weighing contradictory evidence.

Generate the report according to the template in [REPORT_TEMPLATE.md](REPORT_TEMPLATE.md).
Use the evidence levels grid in [EVIDENCE_HIERARCHY.md](EVIDENCE_HIERARCHY.md).

The final report is always in French.

## Rules

- **Minimum 3 independent sources** before any conclusion
- **Never conclude from a single study** — require replication
- **Flag weasel words**: "could", "suggests", "potential" = not proven
- **Prioritize meta-analyses** over isolated studies
- **Mention limitations**: sample size, duration, population studied
- **Search language**: English for scientific databases, French for ANSES/EFSA
- **Graceful degradation**: if an MCP tool or database is unavailable, continue with other sources
