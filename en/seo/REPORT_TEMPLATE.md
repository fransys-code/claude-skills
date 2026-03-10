# SEO Audit Report Template

Adapt according to mode (quick/full/eeat).

---

## Quick mode (Quick Audit)

```markdown
# Quick SEO Verification

Date: [DATE]
Target: [PATH-OR-DIRECTORY]

---

## Executive summary

**General status**: [Excellent / Good / Satisfactory / Needs improvement / Urgent]

**Files verified**: X files (Y excellent, Z needs improvement, W critical)

**Main issue**: [Describe the most impactful problem]

---

## Compliant elements

- [List of pages/elements without problems]

---

## Points to improve

### 1. [Element] - HIGH Priority
**Impact**: [Describe the impact]
**Correction**: [Concrete instruction]
**Example**:
<!-- Before -->
[Existing code]
<!-- After -->
[Corrected code]

### 2. [Element] - MEDIUM Priority

### 3. [Element] - LOW Priority

---

## Immediate recommendations

1. **Action 1**: [Description] — Effort: [Low/Medium/High] — Impact: [Low/Medium/High]
2. **Action 2**: [Description]
3. **Action 3**: [Description]

---

For a more thorough audit:
- `/seo . --mode full` for complete analysis with scoring
- `/seo --mode eeat` for detailed E-E-A-T audit
- `/seo-geo` for AI search engine optimization
```

---

## Full mode (Full Audit)

```markdown
# Complete SEO Audit

**Global score: [XX]/100**

**Evaluation**: [Excellent / Good / Satisfactory / Needs improvement / Urgent]

Date: [DATE]
Project: [ROOT-PATH]
Pages analyzed: [X pages, Y components]
Detected language: [LANGUAGE]
Technology: [Next.js App Router / Pages Router / Other]

---

## Detailed scores by dimension

### 1. Metadata completeness: [XX]/18
- Title: [X/6]
- Meta Description: [X/7]
- Open Graph: [X/3]
- Social Cards: [X/2]

### 2. Structured data: [XX]/13
- JSON-LD: [X/6]
- Mandatory fields: [X/4]
- Validation: [X/3]

### 3. Content quality: [XX]/22
- Heading structure: [X/5]
- Content length: [X/5] — Average: [XXX] words
- Information Gain: [X/4]
- Link quality: [X/3]
- Multimedia: [X/3]
- AI extractability: [X/2]

### 4. E-E-A-T Authority: [XX]/18
- Experience: [X/6]
- Expertise: [X/5]
- Authority: [X/4]
- Trustworthiness: [X/3]

### 5. Content strategy: [XX]/10
- Cluster structure: [X/4]
- Editorial calendar: [X/3]
- Cannibalization: [X/3]

### 6. Technical SEO: [XX]/10
- robots.txt & Sitemap: [X/2]
- Image optimization: [X/2]
- URL structure: [X/2]
- Mobile & Core Web Vitals: [X/3]
- HTTPS: [X/1]

### 7. AI Visibility: [XX]/9
- AI crawlers: [X/3]
- llms.txt: [X/2]
- Extractable content: [X/2]
- Entity linking: [X/2]

---

## Critical issues

### P1 - [Problem 1]
**Description**: [Explanation]
**Affected files**: [Examples]
**Correction**: [Instructions]
**Impact**: [+X points to score]

---

## Suggestions by priority

### HIGH Priority (+3 points or more)
1. [Problem] — Correction: [Instruction] — Effort: [Low/Medium/High]

### MEDIUM Priority (+1-2 points)
1. [Problem] — Correction: [Instruction]

### LOW Priority (+1 point)
1. [Problem] — Correction: [Instruction]

---

## Roadmap

### Week 1-2: Foundation (Gain: +[X] points)
- [ ] [Critical actions]

### Week 3-4: Optimization (Gain: +[Y] points)
- [ ] [Actions]

### Week 5+: Continuous improvement (Gain: +[Z] points)
- [ ] [Long-term actions]

**Projection**: Current score [XX]/100 > Target score [YY]/100
```

---

## E-E-A-T mode

```markdown
# E-E-A-T Audit

**E-E-A-T Score: [XX]/18**

**Sector**: [Sector of activity]
**Sensitivity**: [General / YMYL (health, finance, news)]

Date: [DATE]
Pages analyzed: [X pages]

---

## 1. Experience: [XX]/6
- Case studies: [X present]
- Experience sharing: [Evaluation]
- Technical depth: [Evaluation]

## 2. Expertise: [XX]/5
- Author biography: [Present/Absent]
- Accreditations: [Details]
- Professional language: [Evaluation]

## 3. Authority: [XX]/4
- Unique content: [Evaluation]
- External citations: [Details]

## 4. Trustworthiness: [XX]/3
- Transparency: [About page, legal notices, contact]
- Freshness: [Update frequency]
- Sources: [Number and quality]

---

## Action plan

### Immediate (Week 1-2)
1. [ ] [High-impact action]

### Short term (Month 1)
1. [ ] [Action]

### Long term (Month 2-3)
1. [ ] [Continued action]

**Projection**: Current score [XX]/18 > Target score [YY]/18
```

---

## Filling notes

- `[XX]`: Replace with numeric score
- `[TEXT]`: Replace with relevant content
- Dates in format: DD/MM/YYYY
