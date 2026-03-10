# GEO Scoring System (Generative Engine Optimization)

## Overview

GEO scoring evaluates content's citation potential in AI search engines across 6 dimensions. Each dimension is scored out of 100 points, with a global score calculated as a weighted average.

---

## Scoring Dimensions

### 1. Authority (E-E-A-T) — /100

| Element | Points |
|---------|--------|
| Complete author information | +15 |
| Qualifications and experience mentioned | +15 |
| Clear and numerous citation sources | +15 |
| Publication and modification dates | +10 |
| Organizational affiliations | +10 |
| Entity linking (sameAs to external profiles) | +10 |
| Absence of factual errors | +15 |
| Disclosure of conflicts of interest | +10 |

**Target Score:** 70+

---

### 2. Entity Relationships — /100

| Element | Points |
|---------|--------|
| Explicit definitions of key concepts | +20 |
| Clear heading hierarchy (H1-H6) | +15 |
| Optimal paragraph length (50-150 words) | +15 |
| Internal links to related concepts | +15 |
| Knowledge graph structure | +15 |
| Bulleted/numbered lists | +10 |
| Clarified relationships between entities | +10 |

**Target Score:** 75+

---

### 3. Content Structure — /100

| Element | Points |
|---------|--------|
| Summary/TL;DR at article beginning | +15 |
| FAQ section | +15 |
| Appropriate conceptual density | +15 |
| Headings phrased as questions | +10 |
| Self-contained semantic units (100-300 words) | +10 |
| Logical transitions between sections | +10 |
| Table of contents | +10 |
| Clear and synthetic conclusion | +10 |
| Explicit definition sections | +5 |

**Target Score:** 70+

---

### 4. Data Quality — /100

| Element | Points |
|---------|--------|
| Content accuracy | +20 |
| Freshness (recently updated) | +20 |
| Completeness of subject coverage | +20 |
| Updated statistics with sources | +15 |
| References to recent studies/data | +15 |
| Documented modification log (dateModified) | +10 |

**Target Score:** 80+

---

### 5. Citation Density — /100

| Element | Points |
|---------|--------|
| Number of potentially extractable snippets | +20 |
| Practical utility and actionability | +20 |
| Concrete examples and case studies | +15 |
| Unique and original data (research, benchmarks) | +15 |
| Tools and resources provided | +15 |
| Documented constraints and limitations | +10 |
| Verifiable and attributable claims | +5 |

**Target Score:** 75+

---

### 6. Technical Optimization — /100

| Element | Points |
|---------|--------|
| Schema.org Article present and valid | +25 |
| Schema FAQ if applicable | +15 |
| BreadcrumbList | +10 |
| Complete OpenGraph metadata | +10 |
| H1-H6 tags semantically correct | +10 |
| Optimal title and description | +10 |
| Alt attributes on images | +10 |
| AI crawlers not blocked | +10 |

**Target Score:** 75+

---

## Global Score Calculation

### Standard Weighting

```
Global Score = (Authority x 20% + Entities x 20% + Structure x 20% +
                Data x 20% + Citations x 10% + Technical x 10%)
```

### Interpretation

| Score | Status | Recommendation |
|-------|--------|----------------|
| 80-100 | Excellent | Ready for publication, minimal maintenance |
| 70-79 | Good | Some adjustments recommended |
| 60-69 | Fair | Significant optimizations needed |
| 50-59 | Poor | Partial rewrite recommended |
| 0-49 | Critical | Complete rewrite recommended |

---

## Weighting by AI Engine

Each AI engine has different priorities:

### ChatGPT
| Dimension | Weight |
|-----------|--------|
| Authority | 25% (+5%) |
| Entities | 20% |
| Structure | 20% |
| Data | 20% |
| Citations | 10% |
| Technical | 5% (-5%) |

### Claude
| Dimension | Weight |
|-----------|--------|
| Authority | 15% (-5%) |
| Entities | 30% (+10%) |
| Structure | 25% (+5%) |
| Data | 15% (-5%) |
| Citations | 10% |
| Technical | 5% (-5%) |

### Gemini
| Dimension | Weight |
|-----------|--------|
| Authority | 25% (+5%) |
| Entities | 15% (-5%) |
| Structure | 15% (-5%) |
| Data | 15% (-5%) |
| Citations | 10% |
| Technical | 20% (+10%) |

### Perplexity
| Dimension | Weight |
|-----------|--------|
| Authority | 15% (-5%) |
| Entities | 15% (-5%) |
| Structure | 20% |
| Data | 30% (+10%) |
| Citations | 15% (+5%) |
| Technical | 5% (-5%) |

### Copilot
| Dimension | Weight |
|-----------|--------|
| Authority | 15% (-5%) |
| Entities | 20% |
| Structure | 25% (+5%) |
| Data | 15% (-5%) |
| Citations | 10% |
| Technical | 15% (+5%) |

### Google AI Overviews
| Dimension | Weight |
|-----------|--------|
| Authority | 20% |
| Entities | 15% (-5%) |
| Structure | 15% (-5%) |
| Data | 20% |
| Citations | 10% |
| Technical | 20% (+10%) |

---

## Measurable KPIs

Beyond internal scoring, GEO KPIs to track in production:

| KPI | Description | Tools |
|-----|-------------|-------|
| **AI Citation Share** | Frequency at which the brand is cited in AI responses | SE Ranking, Otterly AI |
| **Citation Sentiment** | Tone (positive/neutral/negative) of mentions in AI responses | Peec AI, manual monitoring |
| **Zero-Click Displacement** | Traffic gained or lost through AI response formats | Google Search Console (AI Overview data) |
| **SERP Feature Presence** | Presence in Featured Snippets, AI Overviews, Knowledge Panels | Semrush, Ahrefs |
| **Brand Mention Frequency** | Brand mentions in third-party sites cited by AIs | SE Ranking |

---

## Expected Improvements by Action

### Quick Actions (impact +5-10 points)

| Action | Estimated Total Impact |
|--------|---------------------|
| Add author Schema + sameAs | +18 |
| Add FAQ Schema (if eligible) | +20 |
| Add TL;DR at article beginning | +11 |
| Update dates (dateModified) | +13 |
| Add sources with links | +17 |

### Priority Actions (impact +10-20 points)

| Action | Estimated Total Impact |
|--------|---------------------|
| Clarify entity relationships and definitions | +23 |
| Add case studies and concrete examples | +25 |
| Restructure into 100-300 word semantic units | +25 |
| Update with recent data | +26 |
| Complete Schema markup (Article + BreadcrumbList) | +25 |
| Phrase headings as questions | +15 |

---

## Targets by Content Type

| Type | Target | Priority |
|------|--------|----------|
| Blog articles | 75+ | Structure, Citations, Entities |
| Product pages | 70+ | Technical, Data |
| Comprehensive guides | 80+ | Authority, Entities, Data |
| FAQ pages | 75+ | Technical, Structure |
| News content | 72+ | Data, Authority, Freshness |
