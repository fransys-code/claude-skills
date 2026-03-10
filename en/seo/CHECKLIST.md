# SEO Audit Checklist

Elements to verify systematically during an SEO audit.

## Metadata

### Title Tag
- [ ] Present on all pages
- [ ] Optimal length (50-60 chars EN, 25-35 chars FR)
- [ ] Contains the main keyword
- [ ] Unique for each page
- [ ] Format: [keyword] | [Brand]

### Meta Description
- [ ] Present on all pages
- [ ] Optimal length (150-160 chars EN, 70-80 chars FR)
- [ ] Contains CTA or relevant context
- [ ] Unique per page

### Open Graph
- [ ] og:title present
- [ ] og:description present
- [ ] og:image present (1200x630px minimum)
- [ ] og:url with canonical
- [ ] og:type correct (article, website, etc.)
- [ ] og:locale if multilingual

### Canonicals & Alternatives
- [ ] Canonical URL declared (rel="canonical")
- [ ] Points to the correct version
- [ ] hreflang if multilingual
- [ ] No redirect chains

### Technical Meta Tags
- [ ] Viewport configured (meta viewport)
- [ ] Charset declared (UTF-8)
- [ ] robots meta if needed (noindex on test pages)

---

## Content

### Heading structure (H1-H6)
- [ ] One H1 per page
- [ ] H1 contains main keyword
- [ ] Logical hierarchy (H1 > H2 > H3, no jumps)
- [ ] H2-H6 contain secondary keywords
- [ ] Descriptive and informative titles

### Text content
- [ ] Minimum length: 800 words (EN) or 600 (FR)
- [ ] Original content providing added value (Information Gain)
- [ ] Original text (no duplicate content)
- [ ] Well-structured paragraphs (< 150 words)
- [ ] Bullet or numbered lists if applicable
- [ ] Self-sufficient passages of 100-300 words (AI Overviews optimization)

### Images and Multimedia
- [ ] All images have descriptive alt text
- [ ] Descriptive file names
- [ ] Optimized compression (< 200KB per image)
- [ ] Modern format (WebP/AVIF with fallback)
- [ ] Lazy loading of images

### Internal links
- [ ] Descriptive anchors (not "click here")
- [ ] Minimum 3-5 relevant internal links
- [ ] Links to pillar pages from related content
- [ ] No broken internal links

### Optimization for AI Overviews and Featured Snippets
- [ ] Questions-answers formatted (H2/H3 sections as questions)
- [ ] Comparative tables if applicable
- [ ] Concise definitions at section start
- [ ] Numbered lists for procedures
- [ ] Extractable passages: direct answer followed by elaboration

---

## Technical

### Site architecture
- [ ] Logical structure (coherent directories)
- [ ] Maximum depth 3-4 levels
- [ ] Clear navigation menu
- [ ] Breadcrumbs present on nested pages

### URLs
- [ ] Short and descriptive URLs
- [ ] Readable format (/blog/seo-audit, not /blog/p=123)
- [ ] HTTPS on all domains
- [ ] 301 redirects if URL changed

### Configuration files
- [ ] robots.txt present and optimized
- [ ] Valid XML sitemap
- [ ] llms.txt present (llmstxt.org standard, optional but recommended)
- [ ] Favicon present

### AI Crawlers
- [ ] GPTBot not blocked in robots.txt (unless deliberate choice)
- [ ] ClaudeBot not blocked
- [ ] PerplexityBot not blocked
- [ ] Google-Extended not blocked
- [ ] Verify Cloudflare configuration (blocks AI bots by default since 2025)

### Mobile & Responsive
- [ ] Responsive test on 375px-1920px
- [ ] Buttons/links clickable (48px minimum)
- [ ] Text readable without zoom
- [ ] Images optimized for mobile

### Performance (Core Web Vitals)
- [ ] Largest Contentful Paint (LCP) < 2s (competitive target)
- [ ] Interaction to Next Paint (INP) < 200ms
- [ ] Cumulative Layout Shift (CLS) < 0.1
- [ ] CSS/JS minification
- [ ] GZIP/Brotli compression enabled

### HTTPS & Security
- [ ] Valid SSL/TLS certificate
- [ ] HTTP > HTTPS redirect
- [ ] No mixed HTTP/HTTPS content

---

## Structured data

### JSON-LD
- [ ] Present on main pages
- [ ] Valid according to Google Rich Results Test
- [ ] Correct type (Article, BlogPosting, WebPage, etc.)

### Schemas by page type

**Articles / Blog:**
- [ ] @type: Article or BlogPosting
- [ ] headline, datePublished, dateModified, author, image

**Products:**
- [ ] @type: Product
- [ ] name, description, image, price, priceCurrency

**Organization:**
- [ ] @type: Organization
- [ ] name, logo, contactPoint, sameAs

**Local Business:**
- [ ] @type: LocalBusiness
- [ ] name, address, telephone, geo, openingHours

### Restrictions and deprecations
- [ ] FAQPage: restricted to government and health sites — do not use elsewhere
- [ ] Q&A: deprecated since January 2026
- [ ] SpecialAnnouncement: deprecated (COVID-specific)
- [ ] Practice Problem: deprecated
- [ ] Sitelinks Search Box: deprecated

---

## E-E-A-T and Credibility

### Experience
- [ ] Case studies or concrete examples
- [ ] Sharing professional experience
- [ ] Demonstrated technical depth

### Expertise
- [ ] Author biography present
- [ ] Accreditations or certifications mentioned
- [ ] Technical language appropriate to domain

### Authority
- [ ] Citations of studies or external sources
- [ ] Original content not duplicate
- [ ] sameAs entity linking to external profiles

### Trustworthiness
- [ ] Complete "About" page
- [ ] Legal notices and privacy policy
- [ ] Clear contact (form, email)
- [ ] Regular update frequency

---

## Correction priority

### Critical (immediate impact)
- Missing Title or Description
- Missing or multiple H1
- Images without alt
- Duplicate content
- Indexation errors (unintended noindex)
- AI crawlers blocked unintentionally

### Important (1-2 weeks)
- Missing Open Graph
- Missing or invalid JSON-LD
- Short content (< 400 words)
- Broken internal links
- Non-optimized mobile

### Recommended (roadmap)
- Complete Social Cards
- Article byline/author bio
- llms.txt for AI crawlers
- Content structured for AI Overviews extraction
