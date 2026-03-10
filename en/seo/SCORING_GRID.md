# SEO Scoring Grid

Scoring system based on 7 dimensions. Total: 100 points.

## 1. Metadata completeness (18 points)

| Element | Points | Criteria |
|---------|--------|----------|
| **Title** | 6 | Present: 2 pts / Optimal length: 2 pts / Target keyword: 2 pts |
| **Meta Description** | 7 | Present: 2 pts / Optimal length: 2 pts / CTA or context: 2 pts / Unique: 1 pt |
| **Open Graph** | 3 | og:title + og:description + og:image: 3 pts |
| **Social Cards** | 2 | og:type + og:url: 2 pts |

**Scoring**: Excellent (16-18) / Good (13-15) / Needs improvement (<13)

---

## 2. Structured data (13 points)

| Element | Points | Criteria |
|---------|--------|----------|
| **JSON-LD present** | 6 | Present and valid: 6 pts / Present but invalid: 3 pts / Absent: 0 pts |
| **Mandatory fields** | 4 | According to schema type (Article, WebPage, Organization, Product) |
| **Validation** | 3 | No warnings: 3 pts / 1-2 warnings: 1 pt / Errors: 0 pts |

Note: Do not penalize absence of FAQPage (restricted to gov/health sites). Do not recommend deprecated schemas.

**Scoring**: Excellent (11-13) / Good (8-10) / Needs improvement (<8)

---

## 3. Content quality (22 points)

| Element | Points | Criteria |
|---------|--------|----------|
| **Heading structure** | 5 | Unique H1: 2 pts / Logical hierarchy: 2 pts / Keywords in headings: 1 pt |
| **Content length** | 5 | EN 800+ = 5 pts, 400-800 = 3 pts / FR 600+ = 5 pts, 300-600 = 3 pts |
| **Information Gain** | 4 | Original content with added value: 2 pts / Unique or proprietary data: 2 pts |
| **Link quality** | 3 | Relevant internal links: 2 pts / Descriptive anchors: 1 pt |
| **Multimedia** | 3 | Images with alt: 1 pt / Videos or graphics: 1 pt / Infographics: 1 pt |
| **AI extractability** | 2 | Self-sufficient passages 100-300 words: 1 pt / Question-answer sections: 1 pt |

**Scoring**: Excellent (18-22) / Good (14-17) / Needs improvement (<14)

---

## 4. E-E-A-T Authority (18 points)

| Element | Points | Criteria |
|---------|--------|----------|
| **Experience** | 6 | Case studies: 2 pts / Real experience: 2 pts / Technical depth: 2 pts |
| **Expertise** | 5 | Author bio with accreditations: 2 pts / Technical precision: 2 pts / Professional language: 1 pt |
| **Authority** | 4 | External citations: 2 pts / Unique and original content: 2 pts |
| **Trustworthiness** | 3 | "About" page: 1 pt / Regular updates: 1 pt / Transparent sources: 1 pt |

YMYL sectors (health, finance): minimum 15/18 recommended.

**Scoring**: Excellent (15-18) / Good (11-14) / Needs improvement (<11)

---

## 5. Content strategy (10 points)

| Element | Points | Criteria |
|---------|--------|----------|
| **Cluster structure** | 4 | Pillar pages: 2 pts / Cluster linking: 2 pts |
| **Editorial calendar** | 3 | Content regularly updated: 2 pts / No orphaned content: 1 pt |
| **Cannibalization** | 3 | No duplication: 3 pts / 1-2 competing pages: 1 pt / 3+: 0 pts |

**Scoring**: Excellent (9-10) / Good (6-8) / Needs improvement (<6)

---

## 6. Technical SEO (10 points)

| Element | Points | Criteria |
|---------|--------|----------|
| **robots.txt & Sitemap** | 2 | Valid robots.txt: 1 pt / XML Sitemap present: 1 pt |
| **Image optimization** | 2 | Complete alt text: 1 pt / Modern formats + compression: 1 pt |
| **URL structure** | 2 | Descriptive URLs: 1 pt / Correct canonicals: 1 pt |
| **Mobile & CWV** | 3 | Valid responsive: 1 pt / LCP < 2s: 1 pt / INP < 200ms + CLS < 0.1: 1 pt |
| **HTTPS** | 1 | HTTPS active and valid: 1 pt |

**Scoring**: Excellent (9-10) / Good (6-8) / Needs improvement (<6)

---

## 7. AI Visibility (9 points)

New dimension evaluating compatibility with AI search engines.

| Element | Points | Criteria |
|---------|--------|----------|
| **AI crawlers not blocked** | 3 | GPTBot, ClaudeBot, PerplexityBot, Google-Extended accessible: 3 pts / Partially: 1 pt |
| **llms.txt** | 2 | Present and conformant to standard: 2 pts / Absent: 0 pts |
| **Extractable content** | 2 | Self-sufficient passages of 100-300 words: 1 pt / Headings as questions: 1 pt |
| **Entity linking** | 2 | sameAs to external profiles: 1 pt / Clearly defined entities: 1 pt |

**Scoring**: Excellent (8-9) / Good (5-7) / Needs improvement (<5)

---

## Global score

**Formula**: Metadata (18) + Structured (13) + Content (22) + E-E-A-T (18) + Strategy (10) + Technical (10) + AI Visibility (9) = /100

| Score | Evaluation |
|-------|-----------|
| **90-100** | Excellent |
| **75-89** | Good |
| **60-74** | Satisfactory |
| **40-59** | Needs improvement |
| **0-39** | Urgent |

---

## Notes by sector

### Health & Finance (YMYL)
- E-E-A-T critical: minimum 15/18
- Verified sources and mandatory citations
- Author bio with accreditations

### E-commerce
- Structured data Product + Review + AggregateRating
- Image optimization and descriptions
- Original content (not copied manufacturer descriptions)

### News & Blog
- Content freshness (prominent dateModified)
- Author and information source
- Content structured for AI Overviews extraction

### B2B & SaaS
- Case studies and social proof
- Pillar pages and topic clusters
- Clear and conversion-oriented CTAs
