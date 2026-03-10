---
name: seo
description: "Audit SEO technique et on-page d'un projet web. Couvre metadonnees, donnees structurees, contenu, E-E-A-T, Core Web Vitals, AI Overviews et scoring. Utiliser cette skill des que l'utilisateur demande un audit SEO, veut verifier ses metadonnees, optimiser son referencement, ameliorer son score technique, ou pose une question sur le SEO d'un site ou d'une page."
user-invocable: true
argument-hint: "[chemin ou URL] [--mode quick|full|eeat] — ou sous-commande : metadata, structured-data, robots-txt, llms-txt"
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

# SEO — Audit & Optimisation On-Page

Audit SEO technique et on-page. Couvre l'analyse technique, le contenu, les metadonnees, les donnees structurees, E-E-A-T, la visibilite IA et les fichiers de configuration.

## Progression

Copier et suivre cette checklist :

```
- [ ] Cible identifiee et mode determine
- [ ] Type de projet detecte (Next.js App/Pages Router, autre)
- [ ] Phase TECHNIQUE : infrastructure et performance analysees
- [ ] Phase CONTENU : structure, qualite et optimisation evaluees
- [ ] Phase METADONNEES : title, description, OG, canonical verifiees
- [ ] Phase STRUCTURED DATA : JSON-LD et schemas valides
- [ ] Phase E-E-A-T : autorite et credibilite evaluees
- [ ] Phase AI VISIBILITY : compatibilite AI Overviews verifiee
- [ ] Phase SYNTHESE : rapport genere avec score et recommandations
```

## 1. Detection et validation

- Analyser `$ARGUMENTS` pour determiner le mode et la cible.
- Si `$ARGUMENTS` est vide, utiliser le repertoire courant en mode quick.
- Detecter le type de projet (Next.js App Router / Pages Router / HTML / autre).
- Detecter la langue du contenu (FR/EN/autre).

**Modes disponibles :**
- `--mode quick` (defaut) : fichier ou repertoire cible, verifications essentielles
- `--mode full` : projet entier, 7 dimensions de scoring (voir [SCORING_GRID.md](SCORING_GRID.md))
- `--mode eeat` : analyse ciblee E-E-A-T (voir [EEAT_RUBRIC.md](EEAT_RUBRIC.md))

**Sous-commandes :**
- `metadata <chemin> [--brand "Marque"]` : genere Title, Meta Description, Open Graph, Canonical
- `structured-data <chemin> [schema-type]` : genere JSON-LD avec detection auto du type Schema.org
- `robots-txt [domain]` : genere un robots.txt optimise
- `llms-txt [--verbose]` : genere un fichier llms.txt (standard llmstxt.org)

## 2. Phase TECHNIQUE — 3 analyses paralleles

Lancer **en parallele via l'outil Agent** :

**Agent A — Infrastructure et configuration**
- Verifier robots.txt, sitemap XML, llms.txt
- Verifier HTTPS, redirections, structure des URLs
- Verifier la configuration mobile (viewport, responsive)

**Agent B — Performance et Core Web Vitals**
- Evaluer LCP (cible < 2s), INP (< 200ms), CLS (< 0.1)
- Verifier minification CSS/JS, compression GZIP/Brotli
- Verifier optimisation images (WebP/AVIF, lazy loading, alt text)
- Si disponible, evaluer les nouveaux signaux : VSI, Engagement Reliability

**Agent C — Compatibilite AI et crawlers**
- Verifier que les crawlers IA ne sont pas bloques (GPTBot, ClaudeBot, PerplexityBot, Google-Extended)
- Attention : Cloudflare bloque les bots IA par defaut depuis 2025
- Verifier la presence de llms.txt et sa conformite au standard llmstxt.org
- Evaluer la structure du contenu pour l'extraction par AI Overviews (passages auto-suffisants de 100-300 mots)

## 3. Phase CONTENU — Analyse de la qualite

- Structure des titres : H1 unique, hierarchie H1-H6 logique
- Longueur du contenu (800+ mots EN, 600+ mots FR)
- Liens internes pertinents avec ancres descriptives
- Images avec alt text, formats modernes
- Contenu original apportant de la valeur ajoutee (signal "Information Gain")
- Structures favorisant les Featured Snippets et AI Overviews : FAQ, tableaux, definitions, listes

## 4. Phase METADONNEES

- Title : present, longueur optimale (50-60 car EN, 25-35 car FR), mot-cle principal
- Meta Description : present, longueur optimale (150-160 car EN, 70-80 car FR), CTA
- Open Graph : og:title, og:description, og:image (1200x630px min), og:url, og:type
- Canonical URL correctement declare
- hreflang si multilingue

## 5. Phase STRUCTURED DATA

- JSON-LD present et valide (format recommande par Google)
- Type correct selon la page (Article, Product, Organization, LocalBusiness, BreadcrumbList, HowTo)
- Champs obligatoires remplis selon le type
- **Restrictions** : FAQPage est restreint aux sites gouvernementaux et de sante
- **Types deprecies depuis janvier 2026** : Q&A, Practice Problem, SpecialAnnouncement, Sitelinks Search Box, Dataset (sauf Dataset Search)
- Validation via Google Rich Results Test

## 6. Phase E-E-A-T

Evaluer selon la grille dans [EEAT_RUBRIC.md](EEAT_RUBRIC.md) :
- **Experience** : etudes de cas, exemples concrets, profondeur technique
- **Expertise** : biographie d'auteur, accreditations, langage professionnel
- **Autorite** : citations externes, contenu unique et original
- **Fiabilite** : page "A propos", mentions legales, contact, frequence de mise a jour

Secteurs YMYL (sante, finance) : exigence renforcee.

## 7. Phase SYNTHESE — ultrathink

Analyser l'ensemble des resultats avec un raisonnement approfondi.

**Mode quick :**
- Resume executif avec etat general
- Points conformes et points a ameliorer par priorite
- Recommandations immediates avec effort/impact

**Mode full :**
- Calculer les scores selon [SCORING_GRID.md](SCORING_GRID.md) (7 dimensions, 100 points)
- Generer le rapport selon [REPORT_TEMPLATE.md](REPORT_TEMPLATE.md)
- Feuille de route par priorite

**Mode eeat :**
- Score E-E-A-T detaille avec plan d'action

Le rapport final est dans la langue du contenu analyse.

## Regles

- **Toujours lire les fichiers avant de juger** — ne pas deviner le contenu
- **Prioriser les problemes critiques** : titre/description manquants, H1 absent, noindex par erreur
- **Distinguer critique / important / recommande** dans les corrections
- **Ne pas recommander FAQPage** sauf sites gouvernementaux ou de sante
- **Ne pas recommander des schemas deprecies** (Q&A, SpecialAnnouncement, etc.)
- **Adapter les seuils** selon la langue detectee (FR/EN ont des longueurs differentes)
- **Graceful degradation** : si un outil n'est pas disponible, continuer avec les autres
- **Pas de references temporelles en dur** — les criteres evoluent, se baser sur les standards actuels

## Fichiers de support

- **[SCORING_GRID.md](SCORING_GRID.md)** : Criteres et baremes de scoring (7 dimensions, 100 points)
- **[CHECKLIST.md](CHECKLIST.md)** : Elements a verifier par categorie
- **[EEAT_RUBRIC.md](EEAT_RUBRIC.md)** : Rubrique d'evaluation E-E-A-T detaillee
- **[REPORT_TEMPLATE.md](REPORT_TEMPLATE.md)** : Structure du rapport de sortie

## Skill connexe

- `/seo-geo` : Optimisation pour moteurs de recherche IA (ChatGPT, Claude, Gemini, Perplexity, AI Overviews)
