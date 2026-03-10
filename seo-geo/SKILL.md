---
name: seo-geo
description: "Optimisation GEO (Generative Engine Optimization) pour les moteurs de recherche IA. Audite le potentiel de citation d'un contenu par ChatGPT, Claude, Gemini, Perplexity, Copilot et Google AI Overviews. Compare avec des concurrents, extrait les entites et reecrit pour maximiser la visibilite IA. Utiliser cette skill des que l'utilisateur veut optimiser son contenu pour les moteurs IA, ameliorer sa visibilite dans les reponses generatives, ou demande comment apparaitre dans ChatGPT, Perplexity ou AI Overviews."
user-invocable: true
argument-hint: "<sous-commande> [args] — sous-commandes : audit, compare, entities, rewrite"
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

Optimisation du contenu pour maximiser la visibilite et les citations dans les moteurs de recherche IA (ChatGPT, Claude, Gemini, Perplexity, Copilot, Google AI Overviews).

## Progression

Copier et suivre cette checklist :

```
- [ ] Sous-commande et cible identifiees
- [ ] Contenu source lu et analyse
- [ ] Phase CRAWLABILITE : accessibilite IA verifiee
- [ ] Phase ANALYSE : 4 dimensions evaluees en parallele
- [ ] Phase SCORING : scores calcules par moteur
- [ ] Phase SYNTHESE : rapport genere avec recommandations actionnables
```

## Sous-commandes

### `audit <fichier-ou-url> [options]`
Analyse le potentiel de citation IA d'un contenu.
- `--detailed` : analyse detaillee
- `--engines <liste>` : moteurs cibles (chatgpt, claude, gemini, perplexity, copilot, ai-overviews)

### `compare <fichier1> <fichier2> [--deep-analysis]`
Compare le potentiel GEO de deux contenus.

### `entities <fichier-ou-url> [--format <type>]`
Extrait les entites cles et genere le balisage Schema.org.

### `rewrite <fichier> [--focus <geo|readability|structure|all>]`
Reecrit le contenu pour ameliorer les scores GEO.

## 1. Lecture du contenu

- Analyser `$ARGUMENTS` pour determiner la sous-commande et la cible.
- Si fichier local : lire avec Read.
- Si URL : fetcher avec WebFetch.
- Si `$ARGUMENTS` est vide, demander via AskUserQuestion.

## 2. Phase CRAWLABILITE — Verification pre-analyse

Avant toute analyse de contenu, verifier que les moteurs IA peuvent acceder au site :

- **robots.txt** : GPTBot, ClaudeBot, PerplexityBot, Googlebot, Google-Extended non bloques ?
- **Cloudflare / WAF** : Cloudflare bloque les bots IA par defaut depuis 2025 — verifier la configuration
- **llms.txt** : present et conforme au standard llmstxt.org ?
- **Rendu** : le contenu est-il dans le HTML source ou genere cote client uniquement (SPA sans SSR) ?

Si des crawlers sont bloques, le signaler comme probleme critique avant de continuer.

## 3. Phase ANALYSE — 4 agents paralleles

Lancer **en parallele via l'outil Agent** :

**Agent A — Autorite et E-E-A-T**
- Informations d'auteur (bio, qualifications, affiliations)
- Sources de citation (nombre, qualite, liens)
- Dates de publication et modification
- Signaux de confiance (page A propos, mentions legales)

**Agent B — Structure et extractibilite**
- Hierarchie des titres (H1-H6)
- Presence d'un resume/TL;DR en debut d'article
- Sections FAQ en fin d'article
- Longueur des paragraphes (cible 50-150 mots)
- Unites semantiques : passages auto-suffisants de 100-300 mots extractibles par les moteurs IA
- Headings formules comme des questions (alignement avec les prompts utilisateur)
- Transitions logiques entre sections

**Agent C — Qualite des donnees et citations**
- Exactitude du contenu (erreurs factuelles ?)
- Fraicheur (derniere mise a jour, donnees recentes)
- Completude de la couverture du sujet
- Donnees uniques ou proprietaires (recherche originale, benchmarks, frameworks)
- Utilite pratique et actionabilite
- Exemples concrets et etudes de cas

**Agent D — Optimisation technique**
- Schema.org present (Article, FAQ, BreadcrumbList)
- Metadonnees OpenGraph completes
- Balises H1-H6 semantiquement correctes
- Attributs alt sur images
- Entity linking (sameAs vers profils et sources externes)

## 4. Phase SCORING

Calculer les scores selon [GEO_SCORING.md](GEO_SCORING.md) :

### Score global (moyenne ponderee des 6 dimensions)
1. Autorite (E-E-A-T)
2. Relations d'entites
3. Structure du contenu
4. Qualite des donnees
5. Densite de citations
6. Optimisation technique

### Scores par moteur IA
Chaque moteur a des poids differents (voir [GEO_SCORING.md](GEO_SCORING.md)) :
- **ChatGPT** : poids fort sur autorite
- **Claude** : poids fort sur structure et relations d'entites
- **Gemini** : poids fort sur autorite et technique (schema, entites)
- **Perplexity** : poids fort sur qualite des donnees et fraicheur
- **Copilot** : poids fort sur structure et technique
- **Google AI Overviews** : poids fort sur technique et E-E-A-T

## 5. Phase SYNTHESE — ultrathink

Analyser l'ensemble des resultats avec un raisonnement approfondi.
Generer le rapport selon [REPORT_TEMPLATE.md](REPORT_TEMPLATE.md).

### Pour `audit`
- Score global + scores par moteur
- Points forts et axes d'amelioration par dimension
- Gains rapides avec impact estime en points
- Plan d'action priorise

### Pour `compare`
- Tableau comparatif dimension par dimension
- Avantages et faiblesses de chaque contenu
- Recommandations pour combler les ecarts

### Pour `entities`
- Liste des entites extraites avec types Schema.org
- Code JSON-LD genere et pret a inserer
- Connexions sameAs recommandees

### Pour `rewrite`
- Contenu reecrit avec les ameliorations appliquees :
  - TL;DR ajoute en debut d'article
  - Definitions explicites des concepts cles
  - Citations de sources avec liens
  - Structure optimisee pour extraction IA (unites semantiques)
  - FAQ en fin d'article
  - Headings en forme de questions

Le rapport final est dans la langue du contenu analyse.

## Regles

- **Penser en prompts, pas en mots-cles** — les utilisateurs posent des questions conversationnelles aux moteurs IA, pas des requetes a mots-cles
- **L'unite de competition GEO est le "claim"** — une phrase verifiable qu'un LLM peut extraire et attribuer
- **Contenu citation-worthy** : recherche originale, donnees proprietaires, frameworks uniques > contenu generique
- **Fraicheur critique** : les moteurs IA privilegient les sources recentes — verifier la date de derniere mise a jour
- **Passages extractibles** : structurer en unites semantiques auto-suffisantes de 100-300 mots
- **SEO reste la fondation** : 99% des citations AI Overviews viennent du top 10 Google — SEO traditionnel + GEO
- **Graceful degradation** : si un outil n'est pas disponible, continuer avec les autres
- **Pas de references temporelles en dur** — les criteres evoluent

## Fichiers de support

- **[GEO_SCORING.md](GEO_SCORING.md)** : Dimensions de scoring, ponderations par moteur et calculs
- **[REPORT_TEMPLATE.md](REPORT_TEMPLATE.md)** : Modele de rapport GEO

## Skill connexe

- `/seo` : Audit SEO traditionnel (technique, metadonnees, E-E-A-T, Core Web Vitals)
