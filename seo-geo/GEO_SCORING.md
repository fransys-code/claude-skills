# Systeme de notation GEO (Generative Engine Optimization)

## Vue d'ensemble

Le scoring GEO evalue le potentiel de citation d'un contenu par les moteurs de recherche IA sur 6 dimensions. Chaque dimension est notee sur 100 points, avec un score global calcule comme moyenne ponderee.

---

## Dimensions de scoring

### 1. Autorite (E-E-A-T) — /100

| Element | Points |
|---------|--------|
| Informations d'auteur completes | +15 |
| Qualifications et experience mentionnees | +15 |
| Sources de citation claires et nombreuses | +15 |
| Dates de publication et modification | +10 |
| Affiliations organisationnelles | +10 |
| Entity linking (sameAs vers profils externes) | +10 |
| Absence d'erreurs factuelles | +15 |
| Divulgation des conflits d'interet | +10 |

**Score cible :** 70+

---

### 2. Relations d'entites — /100

| Element | Points |
|---------|--------|
| Definitions explicites des concepts cles | +20 |
| Hierarchie des titres claire (H1-H6) | +15 |
| Longueur de paragraphes optimale (50-150 mots) | +15 |
| Liens internes vers concepts connexes | +15 |
| Structure du graphe de connaissances | +15 |
| Listes a puces/numerotees | +10 |
| Relations entre entites clarifiees | +10 |

**Score cible :** 75+

---

### 3. Structure du contenu — /100

| Element | Points |
|---------|--------|
| Resume/TL;DR en debut d'article | +15 |
| Section FAQ | +15 |
| Densite conceptuelle appropriee | +15 |
| Headings formules comme des questions | +10 |
| Unites semantiques auto-suffisantes (100-300 mots) | +10 |
| Transitions logiques entre sections | +10 |
| Table des matieres | +10 |
| Conclusion claire et synthetique | +10 |
| Sections de definition explicites | +5 |

**Score cible :** 70+

---

### 4. Qualite des donnees — /100

| Element | Points |
|---------|--------|
| Exactitude du contenu | +20 |
| Fraicheur (mis a jour recemment) | +20 |
| Completude de la couverture du sujet | +20 |
| Statistiques actualisees avec sources | +15 |
| References a des etudes/donnees recentes | +15 |
| Journal des modifications documente (dateModified) | +10 |

**Score cible :** 80+

---

### 5. Densite de citations — /100

| Element | Points |
|---------|--------|
| Nombre de snippets potentiellement extractibles | +20 |
| Utilite pratique et actionabilite | +20 |
| Exemples concrets et etudes de cas | +15 |
| Donnees uniques et originales (recherche, benchmarks) | +15 |
| Outils et ressources fournis | +15 |
| Contraintes et limites documentees | +10 |
| Claims verifiables et attribuables | +5 |

**Score cible :** 75+

---

### 6. Optimisation technique — /100

| Element | Points |
|---------|--------|
| Schema.org Article present et valide | +25 |
| Schema FAQ si applicable | +15 |
| BreadcrumbList | +10 |
| Metadonnees OpenGraph completes | +10 |
| Balises H1-H6 semantiquement correctes | +10 |
| Title et description optimaux | +10 |
| Attributs alt sur images | +10 |
| Crawlers IA non bloques | +10 |

**Score cible :** 75+

---

## Calcul du score global

### Ponderation standard

```
Score Global = (Autorite x 20% + Entites x 20% + Structure x 20% +
                Donnees x 20% + Citations x 10% + Technique x 10%)
```

### Interpretation

| Score | Etat | Recommandation |
|-------|------|----------------|
| 80-100 | Excellent | Pret pour publication, maintenance minimale |
| 70-79 | Bon | Quelques ajustements recommandes |
| 60-69 | Moyen | Optimisations importantes necessaires |
| 50-59 | Faible | Reecriture partiellement recommandee |
| 0-49 | Critique | Reecriture complete recommandee |

---

## Ponderation par moteur IA

Chaque moteur IA a des priorites differentes :

### ChatGPT
| Dimension | Poids |
|-----------|-------|
| Autorite | 25% (+5%) |
| Entites | 20% |
| Structure | 20% |
| Donnees | 20% |
| Citations | 10% |
| Technique | 5% (-5%) |

### Claude
| Dimension | Poids |
|-----------|-------|
| Autorite | 15% (-5%) |
| Entites | 30% (+10%) |
| Structure | 25% (+5%) |
| Donnees | 15% (-5%) |
| Citations | 10% |
| Technique | 5% (-5%) |

### Gemini
| Dimension | Poids |
|-----------|-------|
| Autorite | 25% (+5%) |
| Entites | 15% (-5%) |
| Structure | 15% (-5%) |
| Donnees | 15% (-5%) |
| Citations | 10% |
| Technique | 20% (+10%) |

### Perplexity
| Dimension | Poids |
|-----------|-------|
| Autorite | 15% (-5%) |
| Entites | 15% (-5%) |
| Structure | 20% |
| Donnees | 30% (+10%) |
| Citations | 15% (+5%) |
| Technique | 5% (-5%) |

### Copilot
| Dimension | Poids |
|-----------|-------|
| Autorite | 15% (-5%) |
| Entites | 20% |
| Structure | 25% (+5%) |
| Donnees | 15% (-5%) |
| Citations | 10% |
| Technique | 15% (+5%) |

### Google AI Overviews
| Dimension | Poids |
|-----------|-------|
| Autorite | 20% |
| Entites | 15% (-5%) |
| Structure | 15% (-5%) |
| Donnees | 20% |
| Citations | 10% |
| Technique | 20% (+10%) |

---

## KPIs mesurables

Au-dela du scoring interne, les KPIs GEO a suivre en production :

| KPI | Description | Outils |
|-----|-------------|--------|
| **AI Citation Share** | Frequence a laquelle la marque est citee dans les reponses IA | SE Ranking, Otterly AI |
| **Citation Sentiment** | Ton (positif/neutre/negatif) des mentions dans les reponses IA | Peec AI, monitoring manuel |
| **Zero-Click Displacement** | Trafic gagne ou perdu via les formats de reponse IA | Google Search Console (AI Overview data) |
| **SERP Feature Presence** | Presence dans Featured Snippets, AI Overviews, Knowledge Panels | Semrush, Ahrefs |
| **Brand Mention Frequency** | Mentions de marque dans les sites tiers cites par les IA | SE Ranking |

---

## Ameliorations attendues par action

### Actions rapides (impact +5-10 points)

| Action | Impact total estime |
|--------|---------------------|
| Ajouter auteur Schema + sameAs | +18 |
| Ajouter FAQ Schema (si eligible) | +20 |
| Ajouter TL;DR en debut d'article | +11 |
| Mettre a jour dates (dateModified) | +13 |
| Ajouter sources avec liens | +17 |

### Actions prioritaires (impact +10-20 points)

| Action | Impact total estime |
|--------|---------------------|
| Clarifier relations d'entites et definitions | +23 |
| Ajouter etudes de cas et exemples concrets | +25 |
| Restructurer en unites semantiques de 100-300 mots | +25 |
| Mettre a jour avec donnees recentes | +26 |
| Balisage Schema complet (Article + BreadcrumbList) | +25 |
| Formuler les headings comme des questions | +15 |

---

## Cibles par type de contenu

| Type | Cible | Priorite |
|------|-------|----------|
| Articles de blog | 75+ | Structure, Citations, Entites |
| Pages produit | 70+ | Technique, Donnees |
| Guides complets | 80+ | Autorite, Entites, Donnees |
| Pages FAQ | 75+ | Technique, Structure |
| Contenu d'actualite | 72+ | Donnees, Autorite, Fraicheur |
