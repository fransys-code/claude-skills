---
name: fact-check
description: "Verifies the truthfulness of information, articles, or claims by cross-referencing web sources, scientific papers, fact-checking APIs, and official positions. Covers all domains: science, technology, energy, economics, geopolitics, society. Produces a structured report with verdict, confidence level, and detected biases. Use this skill whenever the user asks if information is true, reliable, exaggerated, or fake, or when they share an article/screenshot to verify."
user-invocable: true
argument-hint: "[URL, text, or description of the claim to verify]"
allowed-tools:
  - Agent
  - Bash
  - Read
  - Write
  - WebSearch
  - WebFetch
  - AskUserQuestion
  - mcp__pubmed__search_pubmed
  - mcp__pubmed__get_paper_fulltext
  - mcp__paper-search__search_pubmed
  - mcp__paper-search__search_arxiv
  - mcp__paper-search__search_google_scholar
  - mcp__paper-search__search_biorxiv
  - mcp__paper-search__search_medrxiv
  - mcp__paper-search__read_pubmed_paper
  - mcp__paper-search__read_arxiv_paper
  - mcp__paper-search__read_biorxiv_paper
  - mcp__paper-search__read_medrxiv_paper
---

# Fact Check

Verifies the truthfulness of information by cross-referencing multiple independent sources, fact-checking APIs, and web research. Produces a structured report with a graded verdict.

## Progress Checklist

Copy and follow this checklist:

```
- [ ] Claim extracted and decomposed into sub-claims
- [ ] Source of origin classified (reliability)
- [ ] PRE-CHECK phase: fact-checking APIs queried
- [ ] RESEARCH phase: 4 parallel searches launched
- [ ] DEEPENING phase: primary sources consulted
- [ ] CROSS-VERIFICATION phase: key facts verified
- [ ] BIAS phase: biases and manipulation detected
- [ ] SYNTHESIS phase: report generated with verdict
```

## 1. Extract the Claim

- Analyze `$ARGUMENTS` (URL, text, or screenshot).
- If it's a URL, fetch the article with WebFetch to extract key claims.
- If it's a screenshot, read the image with Read to extract text.
- If `$ARGUMENTS` is empty, ask the user via `AskUserQuestion`.
- Break down into **verifiable sub-claims** (facts, figures, attributions).

## 2. Classify the Original Source

Evaluate the source of the claim according to the grid in [SOURCE_RELIABILITY.md](SOURCE_RELIABILITY.md):
- Which media/author?
- What type of content (news article, LinkedIn post, tweet, blog, press release)?
- Are sources cited in the original article?

## 3. PRE-CHECK Phase — Fact-checking APIs (NEW)

Before any manual research, query existing fact-check databases **in parallel**:

### 3a. Google Fact Check API

Check if the claim has already been fact-checked by professionals.
See [API_INTEGRATION.md](API_INTEGRATION.md) for technical details.

```bash
# French search
curl -s "https://factchecktools.googleapis.com/v1alpha1/claims:search?query=$(python3 -c 'import urllib.parse; print(urllib.parse.quote("CLAIM_HERE"))')&languageCode=fr&key=${GOOGLE_FACTCHECK_API_KEY}" | python3 -m json.tool

# English search
curl -s "https://factchecktools.googleapis.com/v1alpha1/claims:search?query=$(python3 -c 'import urllib.parse; print(urllib.parse.quote("CLAIM_IN_ENGLISH"))')&languageCode=en&key=${GOOGLE_FACTCHECK_API_KEY}" | python3 -m json.tool
```

If fact-checks already exist: integrate them directly into the report as Tier 2 sources.

**Note**: If the Google Fact Check API key is not configured, skip this phase and move directly to the RESEARCH phase. Do not fail due to missing API key.

## 4. RESEARCH Phase — 4 Parallel Searches

Launch **in parallel via the Agent tool** (one per sub-agent):

**Agent A — Direct fact verification**
- WebSearch: `"[subject] [key fact]"` in French
- WebSearch: `"[subject] [key fact]"` in English
- Search for primary sources cited in the article

**Agent B — Official and institutional sources**
- WebSearch: `"[subject] site:gouv.fr OR site:europa.eu OR site:cnrs.fr OR site:.edu"`
- Search for press releases, official reports, scientific publications

**Agent C — Counter-arguments and debunking**
- WebSearch: `"[subject] fake OR debunk OR exaggerated OR critique OR misleading"`
- Search on fact-checking sites (AFP Factuel, Les Decodeurs, Snopes, Full Fact)

**Agent D — Context and nuance** (if scientific/technical subject)
- If relevant: PubMed / arXiv / Google Scholar searches via MCP paper-search
- WebSearch: `"[subject] nuance OR limitations OR uncertainties OR caveats"`

## 5. DEEPENING Phase — Primary Sources

For each key fact:
- Trace back to the **primary source** (original study, official statement, raw data)
- WebFetch the most relevant sources
- Verify cited figures: are they exact, rounded, distorted?
- Verify citations: are they in context or cherry-picked?

## 6. CROSS-VERIFICATION Phase — Cross-checking

For each sub-claim:
- How many independent sources confirm it?
- Are there contradictions between reliable sources?
- Are figures consistent across sources?
- Are cited experts actually experts in that domain?
- Do API fact-checks (step 3) converge with web searches (step 4)?

## 7. BIAS Phase — Manipulation Detection

Analyze the original article for biases listed in [BIAS_PATTERNS.md](BIAS_PATTERNS.md):
- Sensationalism / clickbait
- Cherry-picking data
- Confusion between correlation and causation
- Omission of important context
- Fallacious appeals to authority
- Figures without reference or out of context

## 8. SYNTHESIS Phase — Deep Reasoning

Analyze all evidence with in-depth reasoning.
Generate the report according to the template in [REPORT_TEMPLATE.md](REPORT_TEMPLATE.md).
Use the verdict grid in [VERDICT_SCALE.md](VERDICT_SCALE.md).

The final report is always in French.

## Rules

- **Minimum 3 independent sources** before any conclusion
- **Always trace back to the primary source** — do not rely on media reprints
- **Distinguish fact, interpretation, and speculation** in the analyzed article
- **Flag sensationalism** even if the substance is true
- **Never say "false" if it's "exaggerated"** — nuance matters
- **Language of research**: French AND English systematically
- **If scientific subject**: also apply science-check criteria (levels of evidence)
- **Graceful degradation**: if an API is unavailable, continue with other sources
