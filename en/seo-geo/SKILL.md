---
name: seo-geo
description: "GEO (Generative Engine Optimization) for AI search engines. Audits the citation potential of content by ChatGPT, Claude, Gemini, Perplexity, Copilot, and Google AI Overviews. Compares with competitors, extracts entities, and rewrites to maximize AI visibility. Use this skill whenever the user wants to optimize content for AI engines, improve visibility in generative responses, or learn how to appear in ChatGPT, Perplexity, or AI Overviews."
user-invocable: true
argument-hint: "<subcommand> [args] — subcommands: audit, compare, entities, rewrite"
allowed-tools:
  - Agent
  - Read
  - Grep
  - Glob
  - Bash
  - WebFetch
  - WebSearch
  - Write
---

# GEO — Generative Engine Optimization

Content optimization to maximize visibility and citations in AI search engines (ChatGPT, Claude, Gemini, Perplexity, Copilot, Google AI Overviews).

## Progression

Copy and follow this checklist:

```
- [ ] Subcommand and target identified
- [ ] Source content read and analyzed
- [ ] CRAWLABILITY phase: AI accessibility verified
- [ ] ANALYSIS phase: 4 dimensions evaluated in parallel
- [ ] SCORING phase: scores calculated per engine
- [ ] SYNTHESIS phase: report generated with actionable recommendations
```

## Subcommands

### `audit <file-or-url> [options]`
Analyzes the AI citation potential of content.
- `--detailed` : detailed analysis
- `--engines <list>` : target engines (chatgpt, claude, gemini, perplexity, copilot, ai-overviews)

### `compare <file1> <file2> [--deep-analysis]`
Compares the GEO potential of two pieces of content.

### `entities <file-or-url> [--format <type>]`
Extracts key entities and generates Schema.org markup.

### `rewrite <file> [--focus <geo|readability|structure|all>]`
Rewrites content to improve GEO scores.

## 1. Reading Content

- Analyze `$ARGUMENTS` to determine the subcommand and target.
- If local file: read with Read.
- If URL: fetch with WebFetch.
- If `$ARGUMENTS` is empty, ask via AskUserQuestion.

## 2. CRAWLABILITY Phase — Pre-analysis Verification

Before any content analysis, verify that AI engines can access the site:

- **robots.txt** : GPTBot, ClaudeBot, PerplexityBot, Googlebot, Google-Extended not blocked?
- **Cloudflare / WAF** : Cloudflare blocks AI bots by default since 2025 — verify configuration
- **llms.txt** : present and compliant with the llmstxt.org standard?
- **Rendering** : is the content in the HTML source or generated client-side only (SPA without SSR)?

If crawlers are blocked, flag this as a critical issue before continuing.

## 3. ANALYSIS Phase — 4 Parallel Agents

Launch **in parallel via the Agent tool**:

**Agent A — Authority and E-E-A-T**
- Author information (bio, qualifications, affiliations)
- Citation sources (number, quality, links)
- Publication and modification dates
- Trust signals (About page, legal mentions)

**Agent B — Structure and Extractability**
- Heading hierarchy (H1-H6)
- Presence of a summary/TL;DR at the beginning of the article
- FAQ sections at the end of the article
- Paragraph length (target 50-150 words)
- Semantic units: self-contained passages of 100-300 words extractable by AI engines
- Headings phrased as questions (alignment with user prompts)
- Logical transitions between sections

**Agent C — Data Quality and Citations**
- Content accuracy (factual errors?)
- Freshness (last update, recent data)
- Completeness of subject coverage
- Unique or proprietary data (original research, benchmarks, frameworks)
- Practical utility and actionability
- Concrete examples and case studies

**Agent D — Technical Optimization**
- Schema.org present (Article, FAQ, BreadcrumbList)
- Complete OpenGraph metadata
- H1-H6 tags semantically correct
- Alt attributes on images
- Entity linking (sameAs to profiles and external sources)

## 4. SCORING Phase

Calculate scores according to [GEO_SCORING.md](GEO_SCORING.md):

### Global score (weighted average of 6 dimensions)
1. Authority (E-E-A-T)
2. Entity relationships
3. Content structure
4. Data quality
5. Citation density
6. Technical optimization

### Scores per AI engine
Each engine has different weights (see [GEO_SCORING.md](GEO_SCORING.md)):
- **ChatGPT** : strong weight on authority
- **Claude** : strong weight on structure and entity relationships
- **Gemini** : strong weight on authority and technical (schema, entities)
- **Perplexity** : strong weight on data quality and freshness
- **Copilot** : strong weight on structure and technical
- **Google AI Overviews** : strong weight on technical and E-E-A-T

## 5. SYNTHESIS Phase — Deep Reasoning

Analyze all results with thorough reasoning.
Generate the report according to [REPORT_TEMPLATE.md](REPORT_TEMPLATE.md).

### For `audit`
- Global score + scores per engine
- Strengths and improvement areas by dimension
- Quick wins with estimated point impact
- Prioritized action plan

### For `compare`
- Comparison table dimension by dimension
- Advantages and weaknesses of each content
- Recommendations to close gaps

### For `entities`
- List of extracted entities with Schema.org types
- Generated JSON-LD code ready to insert
- Recommended sameAs connections

### For `rewrite`
- Content rewritten with applied improvements:
  - TL;DR added at the beginning of the article
  - Explicit definitions of key concepts
  - Source citations with links
  - Structure optimized for AI extraction (semantic units)
  - FAQ at the end of the article
  - Headings phrased as questions

The final report is in the language of the analyzed content.

## Rules

- **Think in prompts, not keywords** — users ask conversational questions to AI engines, not keyword queries
- **The unit of GEO competition is the "claim"** — a verifiable sentence that an LLM can extract and attribute
- **Citation-worthy content** : original research, proprietary data, unique frameworks > generic content
- **Freshness is critical** : AI engines favor recent sources — check last update date
- **Extractable passages** : structure in self-contained semantic units of 100-300 words
- **SEO remains the foundation** : 99% of AI Overviews citations come from Google's top 10 — traditional SEO + GEO
- **Graceful degradation** : if a tool isn't available, continue with others
- **No hard-coded time references** — criteria evolve

## Support Files

- **[GEO_SCORING.md](GEO_SCORING.md)** : Scoring dimensions, engine weights, and calculations
- **[REPORT_TEMPLATE.md](REPORT_TEMPLATE.md)** : GEO report template

## Related Skill

- `/seo` : Traditional SEO audit (technical, metadata, E-E-A-T, Core Web Vitals)
