# Checklist d'audit SEO

Elements a verifier systematiquement lors d'un audit SEO.

## Metadonnees

### Title Tag
- [ ] Present sur toutes les pages
- [ ] Longueur optimale (50-60 car. EN, 25-35 car. FR)
- [ ] Contient le mot-cle principal
- [ ] Unique pour chaque page
- [ ] Format : [mot-cle] | [Brand]

### Meta Description
- [ ] Present sur toutes les pages
- [ ] Longueur optimale (150-160 car. EN, 70-80 car. FR)
- [ ] Contient CTA ou contexte pertinent
- [ ] Unique par page

### Open Graph
- [ ] og:title present
- [ ] og:description present
- [ ] og:image present (1200x630px minimum)
- [ ] og:url avec canonical
- [ ] og:type correct (article, website, etc.)
- [ ] og:locale si multilingue

### Canonicals & Alternatives
- [ ] Canonical URL declare (rel="canonical")
- [ ] Pointe vers la version correcte
- [ ] hreflang si multilingue
- [ ] Pas de chaines de redirections

### Technical Meta Tags
- [ ] Viewport configure (meta viewport)
- [ ] Charset declare (UTF-8)
- [ ] robots meta si besoin (noindex sur pages test)

---

## Contenu

### Structure des titres (H1-H6)
- [ ] Un seul H1 par page
- [ ] H1 contient mot-cle principal
- [ ] Hierarchie logique (H1 > H2 > H3, pas de sauts)
- [ ] H2-H6 contiennent mots-cles secondaires
- [ ] Titres descriptifs et informatifs

### Contenu textuel
- [ ] Longueur minimale : 800 mots (EN) ou 600 (FR)
- [ ] Contenu original apportant une valeur ajoutee (Information Gain)
- [ ] Texte original (pas de contenu duplique)
- [ ] Paragraphes bien structures (< 150 mots)
- [ ] Listes a puces ou numerotees si applicable
- [ ] Passages auto-suffisants de 100-300 mots (optimisation AI Overviews)

### Images et Multimedia
- [ ] Toutes images ont texte alt descriptif
- [ ] Noms de fichiers descriptifs
- [ ] Compression optimisee (< 200KB par image)
- [ ] Format moderne (WebP/AVIF avec fallback)
- [ ] Lazy loading des images

### Liens internes
- [ ] Ancres descriptives (pas "cliquez ici")
- [ ] Au minimum 3-5 liens internes pertinents
- [ ] Liens vers pages piliers depuis contenu connexe
- [ ] Pas de liens casses internes

### Optimisation pour AI Overviews et Featured Snippets
- [ ] Questions-reponses formatees (sections H2/H3 en forme de question)
- [ ] Tableaux comparatifs si applicable
- [ ] Definitions concises en debut de section
- [ ] Listes numerotees pour procedures
- [ ] Passages extractibles : reponse directe suivie d'elaboration

---

## Technique

### Architecture du site
- [ ] Structure logique (repertoires coherents)
- [ ] Profondeur max 3-4 niveaux
- [ ] Menu de navigation clair
- [ ] Breadcrumbs presents sur pages imbriquees

### URLs
- [ ] URLs courtes et descriptives
- [ ] Format lisible (/blog/seo-audit, pas /blog/p=123)
- [ ] HTTPS sur tous les domaines
- [ ] Redirections 301 si changement d'URL

### Fichiers de configuration
- [ ] robots.txt present et optimise
- [ ] Sitemap XML valide
- [ ] llms.txt present (standard llmstxt.org, optionnel mais recommande)
- [ ] Favicon present

### Crawlers IA
- [ ] GPTBot non bloque dans robots.txt (sauf choix delibere)
- [ ] ClaudeBot non bloque
- [ ] PerplexityBot non bloque
- [ ] Google-Extended non bloque
- [ ] Verifier configuration Cloudflare (bloque les bots IA par defaut depuis 2025)

### Mobile & Responsive
- [ ] Test responsive sur 375px-1920px
- [ ] Boutons/liens cliquables (48px minimum)
- [ ] Texte lisible sans zoom
- [ ] Images optimisees pour mobile

### Performance (Core Web Vitals)
- [ ] Largest Contentful Paint (LCP) < 2s (cible competitive)
- [ ] Interaction to Next Paint (INP) < 200ms
- [ ] Cumulative Layout Shift (CLS) < 0.1
- [ ] Minification CSS/JS
- [ ] Compression GZIP/Brotli activee

### HTTPS & Securite
- [ ] Certificat SSL/TLS valide
- [ ] Redirection HTTP > HTTPS
- [ ] Pas de contenu mixte HTTP/HTTPS

---

## Donnees structurees

### JSON-LD
- [ ] Present sur pages principales
- [ ] Valide selon Google Rich Results Test
- [ ] Type correct (Article, BlogPosting, WebPage, etc.)

### Schemas selon type de page

**Articles / Blog :**
- [ ] @type: Article ou BlogPosting
- [ ] headline, datePublished, dateModified, author, image

**Produits :**
- [ ] @type: Product
- [ ] name, description, image, price, priceCurrency

**Organisation :**
- [ ] @type: Organization
- [ ] name, logo, contactPoint, sameAs

**Local Business :**
- [ ] @type: LocalBusiness
- [ ] name, address, telephone, geo, openingHours

### Restrictions et deprecations
- [ ] FAQPage : restreint aux sites gouvernementaux et de sante — ne pas utiliser ailleurs
- [ ] Q&A : deprecie depuis janvier 2026
- [ ] SpecialAnnouncement : deprecie (COVID-specifique)
- [ ] Practice Problem : deprecie
- [ ] Sitelinks Search Box : deprecie

---

## E-E-A-T et Credibilite

### Experience
- [ ] Etudes de cas ou exemples concrets
- [ ] Partage d'experience professionnelle
- [ ] Profondeur technique demontree

### Expertise
- [ ] Biographie d'auteur presente
- [ ] Accreditations ou certifications mentionnees
- [ ] Langage technique approprie au domaine

### Autorite
- [ ] Citations d'etudes ou sources externes
- [ ] Contenu original non duplique
- [ ] Entite sameAs reliant aux profils externes

### Fiabilite
- [ ] Page "A propos" complete
- [ ] Mentions legales et politique de confidentialite
- [ ] Contact clair (formulaire, email)
- [ ] Frequence de mise a jour reguliere

---

## Priorite de correction

### Critique (impact immediat)
- Title ou Description manquants
- H1 absent ou multiple
- Images sans alt
- Contenu duplique
- Erreurs d'indexation (noindex par erreur)
- Crawlers IA bloques involontairement

### Important (1-2 semaines)
- Open Graph manquants
- JSON-LD absent ou invalide
- Contenu court (< 400 mots)
- Liens casses internes
- Mobile non-optimise

### Recommande (roadmap)
- Social Cards completes
- Article byline/author bio
- llms.txt pour crawlers IA
- Contenu structure pour extraction AI Overviews
