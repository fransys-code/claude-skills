---
name: fact-check
description: "Verifie la veracite d'une information, article ou affirmation en croisant sources web, papers scientifiques, APIs de fact-checking et positions officielles. Couvre tous les domaines : science, technologie, energie, economie, geopolitique, societe. Produit un rapport structure avec verdict, niveau de confiance et biais detectes. Utiliser cette skill des que l'utilisateur demande si une info est vraie, fiable, exageree ou fake, ou quand il partage un article/screenshot a verifier."
user-invocable: true
argument-hint: "[URL, texte ou description de l'affirmation a verifier]"
allowed-tools:
  - Agent
  - Bash
  - Read
  - Write
  - WebSearch
  - WebFetch
  - AskUserQuestion
  - mcp__pubmed__search_pubmed
  - mcp__pubmed__get_paper_fulltext
  - mcp__paper-search__search_pubmed
  - mcp__paper-search__search_arxiv
  - mcp__paper-search__search_google_scholar
  - mcp__paper-search__search_biorxiv
  - mcp__paper-search__search_medrxiv
  - mcp__paper-search__read_pubmed_paper
  - mcp__paper-search__read_arxiv_paper
  - mcp__paper-search__read_biorxiv_paper
  - mcp__paper-search__read_medrxiv_paper
---

# Fact Check

Verifie la veracite d'une information en croisant plusieurs sources independantes, APIs de fact-checking et recherches web. Produit un rapport structure avec verdict gradue.

## Progression

Copier et suivre cette checklist :

```
- [ ] Affirmation extraite et decomposee en sous-affirmations
- [ ] Source d'origine classifiee (fiabilite)
- [ ] Phase PRE-CHECK : APIs fact-checking interrogees
- [ ] Phase RECHERCHE : 4 recherches paralleles lancees
- [ ] Phase APPROFONDISSEMENT : sources primaires consultees
- [ ] Phase CROISEMENT : verification croisee des faits cles
- [ ] Phase BIAIS : detection des biais et manipulation
- [ ] Phase SYNTHESE : rapport genere avec verdict
```

## 1. Extraire l'affirmation

- Analyser `$ARGUMENTS` (URL, texte, ou screenshot).
- Si c'est une URL, fetcher l'article avec WebFetch pour en extraire les affirmations cles.
- Si c'est un screenshot, lire l'image avec Read pour en extraire le texte.
- Si `$ARGUMENTS` est vide, demander a l'utilisateur via `AskUserQuestion`.
- Decomposer en **sous-affirmations verifiables** (faits, chiffres, attributions).

## 2. Classifier la source d'origine

Evaluer la source de l'affirmation selon la grille dans [SOURCE_RELIABILITY.md](SOURCE_RELIABILITY.md) :
- Quel media/auteur ?
- Quel type de contenu (article journalistique, post LinkedIn, tweet, blog, communique de presse) ?
- Presence de sources citees dans l'article original ?

## 3. Phase PRE-CHECK — APIs de fact-checking (NOUVEAU)

Avant toute recherche manuelle, interroger les bases de fact-checks existants **en parallele** :

### 3a. Google Fact Check API

Verifier si l'affirmation a deja ete fact-checkee par des professionnels.
Voir [API_INTEGRATION.md](API_INTEGRATION.md) pour les details techniques.

```bash
# Recherche FR
curl -s "https://factchecktools.googleapis.com/v1alpha1/claims:search?query=$(python3 -c 'import urllib.parse; print(urllib.parse.quote("AFFIRMATION_ICI"))')&languageCode=fr&key=${GOOGLE_FACTCHECK_API_KEY}" | python3 -m json.tool

# Recherche EN
curl -s "https://factchecktools.googleapis.com/v1alpha1/claims:search?query=$(python3 -c 'import urllib.parse; print(urllib.parse.quote("CLAIM_IN_ENGLISH"))')&languageCode=en&key=${GOOGLE_FACTCHECK_API_KEY}" | python3 -m json.tool
```

Si des fact-checks existent deja : les integrer directement dans le rapport comme source Tier 2.

**Note** : Si la cle API Google Fact Check n'est pas configuree, sauter cette phase et passer directement a la phase RECHERCHE. Ne pas echouer sur l'absence de cle.

## 4. Phase RECHERCHE — 4 recherches paralleles

Lancer **en parallele via l'outil Agent** (une par sous-agent) :

**Agent A — Verification factuelle directe**
- WebSearch: `"[sujet] [fait cle]"` en francais
- WebSearch: `"[sujet] [key fact]"` en anglais
- Chercher les sources primaires citees dans l'article

**Agent B — Sources officielles et institutionnelles**
- WebSearch: `"[sujet] site:gouv.fr OR site:europa.eu OR site:cnrs.fr OR site:.edu"`
- Chercher communiques de presse, rapports officiels, publications scientifiques

**Agent C — Contre-arguments et debunking**
- WebSearch: `"[sujet] fake OR debunk OR exagere OR critique OR misleading"`
- Chercher sur les sites de fact-checking (AFP Factuel, Les Decodeurs, Snopes, Full Fact)

**Agent D — Contexte et nuance** (si sujet scientifique/technique)
- Si pertinent : recherches PubMed / arXiv / Google Scholar via MCP paper-search
- WebSearch: `"[sujet] nuance OR limites OR incertitudes OR caveats"`

## 5. Phase APPROFONDISSEMENT — Sources primaires

Pour chaque fait cle :
- Remonter a la **source primaire** (etude originale, communique officiel, donnees brutes)
- WebFetch les sources les plus pertinentes
- Verifier les chiffres cites : sont-ils exacts, arrondis, deformes ?
- Verifier les citations : sont-elles en contexte ou cherry-pickees ?

## 6. Phase CROISEMENT — Verification croisee

Pour chaque sous-affirmation :
- Combien de sources independantes la confirment ?
- Y a-t-il des contradictions entre sources fiables ?
- Les chiffres sont-ils coherents entre les sources ?
- Les experts cites sont-ils reellement experts du domaine ?
- Les fact-checks APIs (etape 3) convergent-ils avec les recherches web (etape 4) ?

## 7. Phase BIAIS — Detection des manipulations

Analyser l'article original pour detecter les biais listes dans [BIAS_PATTERNS.md](BIAS_PATTERNS.md) :
- Sensationnalisme / clickbait
- Cherry-picking de donnees
- Confusion correlation/causalite
- Omission de contexte important
- Appel a l'autorite fallacieux
- Chiffres sans reference ou hors contexte

## 8. Phase SYNTHESE — ultrathink

Analyser l'ensemble des preuves avec un raisonnement approfondi.
Generer le rapport selon le template dans [REPORT_TEMPLATE.md](REPORT_TEMPLATE.md).
Utiliser la grille de verdicts dans [VERDICT_SCALE.md](VERDICT_SCALE.md).

Le rapport final est toujours en francais.

## Regles

- **Minimum 3 sources independantes** avant toute conclusion
- **Toujours remonter a la source primaire** — ne pas se fier aux reprises media
- **Distinguer fait, interpretation et speculation** dans l'article analyse
- **Signaler le sensationnalisme** meme si le fond est vrai
- **Ne jamais dire "faux" si c'est "exagere"** — la nuance compte
- **Langue des recherches** : francais ET anglais systematiquement
- **Si sujet scientifique** : appliquer aussi les criteres de science-check (niveaux de preuve)
- **Graceful degradation** : si une API est indisponible, continuer avec les autres sources
