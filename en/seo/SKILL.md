---
name: seo
description: "Technical and on-page SEO audit of a web project. Covers metadata, structured data, content, E-E-A-T, Core Web Vitals, AI Overviews and scoring. Use this skill whenever the user requests an SEO audit, wants to verify their metadata, optimize their referencing, improve their technical score, or asks a question about the SEO of a site or page."
user-invocable: true
argument-hint: "[path or URL] [--mode quick|full|eeat] — or sub-command : metadata, structured-data, robots-txt, llms-txt"
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

# SEO — Audit & On-Page Optimization

Technical and on-page SEO audit. Covers technical analysis, content, metadata, structured data, E-E-A-T, AI visibility and configuration files.

## Progress

Copy and follow this checklist:

```
- [ ] Target identified and mode determined
- [ ] Project type detected (Next.js App/Pages Router, other)
- [ ] TECHNICAL phase: infrastructure and performance analyzed
- [ ] CONTENT phase: structure, quality and optimization evaluated
- [ ] METADATA phase: title, description, OG, canonical verified
- [ ] STRUCTURED DATA phase: JSON-LD and schemas validated
- [ ] E-E-A-T phase: authority and credibility evaluated
- [ ] AI VISIBILITY phase: AI Overviews compatibility verified
- [ ] SYNTHESIS phase: report generated with score and recommendations
```

## 1. Detection and validation

- Analyze `$ARGUMENTS` to determine the mode and target.
- If `$ARGUMENTS` is empty, use the current directory in quick mode.
- Detect project type (Next.js App Router / Pages Router / HTML / other).
- Detect content language (FR/EN/other).

**Available modes:**
- `--mode quick` (default): target file or directory, essential verifications
- `--mode full`: entire project, 7 scoring dimensions (see [SCORING_GRID.md](SCORING_GRID.md))
- `--mode eeat`: targeted E-E-A-T analysis (see [EEAT_RUBRIC.md](EEAT_RUBRIC.md))

**Sub-commands:**
- `metadata <path> [--brand "Brand"]`: generates Title, Meta Description, Open Graph, Canonical
- `structured-data <path> [schema-type]`: generates JSON-LD with auto-detection of Schema.org type
- `robots-txt [domain]`: generates an optimized robots.txt
- `llms-txt [--verbose]`: generates an llms.txt file (llmstxt.org standard)

## 2. TECHNICAL phase — 3 parallel analyses

Launch **in parallel via Agent tool**:

**Agent A — Infrastructure and configuration**
- Verify robots.txt, XML sitemap, llms.txt
- Verify HTTPS, redirects, URL structure
- Verify mobile configuration (viewport, responsive)

**Agent B — Performance and Core Web Vitals**
- Evaluate LCP (target < 2s), INP (< 200ms), CLS (< 0.1)
- Verify CSS/JS minification, GZIP/Brotli compression
- Verify image optimization (WebP/AVIF, lazy loading, alt text)
- If available, evaluate new signals: VSI, Engagement Reliability

**Agent C — AI and crawler compatibility**
- Verify that AI crawlers are not blocked (GPTBot, ClaudeBot, PerplexityBot, Google-Extended)
- Note: Cloudflare blocks AI bots by default since 2025
- Verify the presence of llms.txt and its conformance to the llmstxt.org standard
- Evaluate content structure for extraction by AI Overviews (self-sufficient passages of 100-300 words)

## 3. CONTENT phase — Quality analysis

- Heading structure: unique H1, logical H1-H6 hierarchy
- Content length (800+ words EN, 600+ words FR)
- Relevant internal links with descriptive anchors
- Images with alt text, modern formats
- Original content providing added value (signal "Information Gain")
- Structures favoring Featured Snippets and AI Overviews: FAQ, tables, definitions, lists

## 4. METADATA phase

- Title: present, optimal length (50-60 chars EN, 25-35 chars FR), main keyword
- Meta Description: present, optimal length (150-160 chars EN, 70-80 chars FR), CTA
- Open Graph: og:title, og:description, og:image (1200x630px min), og:url, og:type
- Canonical URL correctly declared
- hreflang if multilingual

## 5. STRUCTURED DATA phase

- JSON-LD present and valid (Google recommended format)
- Correct type according to page (Article, Product, Organization, LocalBusiness, BreadcrumbList, HowTo)
- Mandatory fields filled according to type
- **Restrictions**: FAQPage is restricted to government and health sites
- **Deprecated types since January 2026**: Q&A, Practice Problem, SpecialAnnouncement, Sitelinks Search Box, Dataset (except Dataset Search)
- Validation via Google Rich Results Test

## 6. E-E-A-T phase

Evaluate according to the rubric in [EEAT_RUBRIC.md](EEAT_RUBRIC.md):
- **Experience**: case studies, concrete examples, technical depth
- **Expertise**: author biography, accreditations, professional language
- **Authority**: external citations, unique and original content
- **Trustworthiness**: "About" page, legal notices, contact, update frequency

YMYL sectors (health, finance): reinforced requirement.

## 7. SYNTHESIS phase — ultrathink

Analyze all results with deep reasoning.

**Quick mode:**
- Executive summary with general status
- Compliant points and improvement points by priority
- Immediate recommendations with effort/impact

**Full mode:**
- Calculate scores according to [SCORING_GRID.md](SCORING_GRID.md) (7 dimensions, 100 points)
- Generate report according to [REPORT_TEMPLATE.md](REPORT_TEMPLATE.md)
- Roadmap by priority

**EEAT mode:**
- Detailed E-E-A-T score with action plan

The final report is in the language of the analyzed content.

## Rules

- **Always read files before judging** — do not guess content
- **Prioritize critical problems**: missing title/description, missing H1, unintended noindex
- **Distinguish critical / important / recommended** in corrections
- **Do not recommend FAQPage** except for government or health sites
- **Do not recommend deprecated schemas** (Q&A, SpecialAnnouncement, etc.)
- **Adapt thresholds** according to detected language (FR/EN have different lengths)
- **Graceful degradation**: if a tool is not available, continue with others
- **No hard-coded temporal references** — criteria evolve, base on current standards

## Support files

- **[SCORING_GRID.md](SCORING_GRID.md)**: Scoring criteria and scales (7 dimensions, 100 points)
- **[CHECKLIST.md](CHECKLIST.md)**: Elements to verify by category
- **[EEAT_RUBRIC.md](EEAT_RUBRIC.md)**: Detailed E-E-A-T evaluation rubric
- **[REPORT_TEMPLATE.md](REPORT_TEMPLATE.md)**: Output report structure

## Related skill

- `/seo-geo`: Optimization for AI search engines (ChatGPT, Claude, Gemini, Perplexity, AI Overviews)
