# Grille de Scoring SEO

Systeme de scoring sur 7 dimensions. Total : 100 points.

## 1. Completude des metadonnees (18 points)

| Element | Points | Criteres |
|---------|--------|----------|
| **Title** | 6 | Present : 2 pts / Longueur optimale : 2 pts / Mot-cle cible : 2 pts |
| **Meta Description** | 7 | Present : 2 pts / Longueur optimale : 2 pts / CTA ou contexte : 2 pts / Unique : 1 pt |
| **Open Graph** | 3 | og:title + og:description + og:image : 3 pts |
| **Social Cards** | 2 | og:type + og:url : 2 pts |

**Scoring** : Excellent (16-18) / Bon (13-15) / A ameliorer (<13)

---

## 2. Donnees structurees (13 points)

| Element | Points | Criteres |
|---------|--------|----------|
| **JSON-LD present** | 6 | Present et valide : 6 pts / Present mais invalide : 3 pts / Absent : 0 pts |
| **Champs obligatoires** | 4 | Selon type de schema (Article, WebPage, Organization, Product) |
| **Validation** | 3 | Aucun avertissement : 3 pts / 1-2 avertissements : 1 pt / Erreurs : 0 pts |

Note : Ne pas penaliser l'absence de FAQPage (restreint aux sites gouv/sante). Ne pas recommander de schemas deprecies.

**Scoring** : Excellent (11-13) / Bon (8-10) / A ameliorer (<8)

---

## 3. Qualite du contenu (22 points)

| Element | Points | Criteres |
|---------|--------|----------|
| **Structure des titres** | 5 | H1 unique : 2 pts / Hierarchie logique : 2 pts / Mots-cles dans titres : 1 pt |
| **Longueur du contenu** | 5 | EN 800+ = 5 pts, 400-800 = 3 pts / FR 600+ = 5 pts, 300-600 = 3 pts |
| **Information Gain** | 4 | Contenu original avec valeur ajoutee : 2 pts / Donnees uniques ou proprietary : 2 pts |
| **Qualite des liens** | 3 | Liens internes pertinents : 2 pts / Ancres descriptives : 1 pt |
| **Multimedia** | 3 | Images avec alt : 1 pt / Videos ou graphiques : 1 pt / Infographies : 1 pt |
| **Extractibilite IA** | 2 | Passages auto-suffisants 100-300 mots : 1 pt / Sections question-reponse : 1 pt |

**Scoring** : Excellent (18-22) / Bon (14-17) / A ameliorer (<14)

---

## 4. Autorite E-E-A-T (18 points)

| Element | Points | Criteres |
|---------|--------|----------|
| **Experience** | 6 | Etudes de cas : 2 pts / Experience reelle : 2 pts / Profondeur technique : 2 pts |
| **Expertise** | 5 | Bio d'auteur avec accreditations : 2 pts / Precision technique : 2 pts / Langage pro : 1 pt |
| **Autorite** | 4 | Citations externes : 2 pts / Contenu unique et original : 2 pts |
| **Fiabilite** | 3 | Page "A propos" : 1 pt / Mise a jour reguliere : 1 pt / Sources transparentes : 1 pt |

Secteurs YMYL (sante, finance) : minimum 15/18 recommande.

**Scoring** : Excellent (15-18) / Bon (11-14) / A ameliorer (<11)

---

## 5. Strategie de contenu (10 points)

| Element | Points | Criteres |
|---------|--------|----------|
| **Structure en clusters** | 4 | Pages piliers : 2 pts / Liens en clusters : 2 pts |
| **Calendrier editorial** | 3 | Contenu regulierement mis a jour : 2 pts / Pas de contenu orphelin : 1 pt |
| **Cannibalisation** | 3 | Aucune duplication : 3 pts / 1-2 pages en competition : 1 pt / 3+ : 0 pts |

**Scoring** : Excellent (9-10) / Bon (6-8) / A ameliorer (<6)

---

## 6. SEO technique (10 points)

| Element | Points | Criteres |
|---------|--------|----------|
| **robots.txt & Sitemap** | 2 | robots.txt valide : 1 pt / Sitemap XML present : 1 pt |
| **Optimisation images** | 2 | Alt text complet : 1 pt / Formats modernes + compression : 1 pt |
| **Structure URLs** | 2 | URLs descriptives : 1 pt / Canonicals corrects : 1 pt |
| **Mobile & CWV** | 3 | Responsive valide : 1 pt / LCP < 2s : 1 pt / INP < 200ms + CLS < 0.1 : 1 pt |
| **HTTPS** | 1 | HTTPS actif et valide : 1 pt |

**Scoring** : Excellent (9-10) / Bon (6-8) / A ameliorer (<6)

---

## 7. Visibilite IA (9 points)

Nouvelle dimension evaluant la compatibilite avec les moteurs de recherche IA.

| Element | Points | Criteres |
|---------|--------|----------|
| **Crawlers IA non bloques** | 3 | GPTBot, ClaudeBot, PerplexityBot, Google-Extended accessibles : 3 pts / Partiellement : 1 pt |
| **llms.txt** | 2 | Present et conforme au standard : 2 pts / Absent : 0 pts |
| **Contenu extractible** | 2 | Passages auto-suffisants de 100-300 mots : 1 pt / Headings en forme de questions : 1 pt |
| **Entity linking** | 2 | sameAs vers profils externes : 1 pt / Entites clairement definies : 1 pt |

**Scoring** : Excellent (8-9) / Bon (5-7) / A ameliorer (<5)

---

## Score global

**Formule** : Metadonnees (18) + Structurees (13) + Contenu (22) + E-E-A-T (18) + Strategie (10) + Technique (10) + Visibilite IA (9) = /100

| Score | Evaluation |
|-------|-----------|
| **90-100** | Excellent |
| **75-89** | Bon |
| **60-74** | Satisfaisant |
| **40-59** | A ameliorer |
| **0-39** | Urgent |

---

## Notes par secteur

### Sante & Finance (YMYL)
- E-E-A-T critique : minimum 15/18
- Sources verifiees et citations obligatoires
- Bio d'auteur avec accreditations

### E-commerce
- Donnees structurees Product + Review + AggregateRating
- Optimisation images et descriptions
- Contenu original (pas de descriptions fabricant copiees)

### Actualites & Blog
- Fraicheur du contenu (dateModified prominente)
- Auteur et source d'information
- Contenu structure pour extraction AI Overviews

### B2B & SaaS
- Etudes de cas et preuves sociales
- Pages piliers et topic clusters
- CTAs clairs et conversion-oriented
