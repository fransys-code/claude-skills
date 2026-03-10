---
name: science-check
description: "Verifie une affirmation scientifique ou de sante en croisant PubMed, Semantic Scholar, et le web. Produit un rapport structure avec niveaux de preuve, risques et positions officielles. Utiliser cette skill des que l'utilisateur pose une question sur la sante, la nutrition, un complement alimentaire, un medicament, une therapie, ou demande si une affirmation scientifique est vraie, prouvee ou fiable, meme si la question est informelle."
user-invocable: true
argument-hint: "[affirmation a verifier]"
allowed-tools:
  - Agent
  - Bash
  - Read
  - WebSearch
  - WebFetch
  - AskUserQuestion
  - Write
  - mcp__pubmed__search_pubmed
  - mcp__pubmed__get_paper_fulltext
  - mcp__paper-search__search_pubmed
  - mcp__paper-search__search_arxiv
  - mcp__paper-search__search_google_scholar
  - mcp__paper-search__search_biorxiv
  - mcp__paper-search__search_medrxiv
  - mcp__paper-search__read_pubmed_paper
  - mcp__paper-search__read_biorxiv_paper
  - mcp__paper-search__read_medrxiv_paper
---

# Science Check

Verifie ce que dit la science sur une affirmation donnee, en croisant plusieurs sources et en produisant un rapport structure.

## Progression

Copier et suivre cette checklist :

```
- [ ] Affirmation recuperee et traduite en anglais
- [ ] Phase ORIENTATION : 3 recherches paralleles lancees
- [ ] Phase APPROFONDISSEMENT : sources de reference fetchees
- [ ] Phase VALIDATION : etudes cles verifiees
- [ ] Phase AUTO-VERIFICATION : checklist qualite validee
- [ ] Phase SYNTHESE : rapport genere
```

## 1. Recuperer l'affirmation

- Si `$ARGUMENTS` est fourni, l'utiliser comme affirmation a verifier.
- Sinon, demander a l'utilisateur via `AskUserQuestion`.
- Formuler la requete de recherche en anglais.

## 2. Phase ORIENTATION — 3 recherches paralleles

Lancer les 3 recherches **en parallele via l'outil Agent** (une par sous-agent) :

**Agent A — Meta-analyses et revues systematiques**
- WebSearch: `"[sujet] systematic review meta-analysis site:pubmed.ncbi.nlm.nih.gov OR site:cochranelibrary.com"`
- Si MCP pubmed/paper-search disponible : chercher avec filtre "meta-analysis" ou "systematic review"

**Agent B — Risques et effets negatifs**
- WebSearch: `"[sujet] risks side effects adverse scientific evidence"`

**Agent C — Position critique / debunking**
- WebSearch: `"[sujet] claims debunked OR confirmed OR evidence-based science"`

## 3. Phase APPROFONDISSEMENT — Sources de reference

Fetcher les meilleures sources trouvees, en privilegiant les sources fiables listees dans [TRUSTED_SOURCES.md](TRUSTED_SOURCES.md).

Si MCP paper-search disponible : rechercher sur Semantic Scholar / Google Scholar pour les citations et l'impact.

## 4. Phase VALIDATION — Verification croisee

Pour les 3-5 etudes cles identifiees :
- Taille d'echantillon (n=?)
- Type d'etude (RCT, observationnelle, animale, in vitro)
- Financement (industrie = biais potentiel)
- Replication des resultats
- Verifier via WebSearch si retractees (Retraction Watch)

## 5. Phase AUTO-VERIFICATION — Checklist qualite

Avant de rediger le rapport, verifier :
- [ ] Au moins 3 sources independantes consultees
- [ ] Au moins 1 meta-analyse ou revue systematique trouvee (sinon le signaler dans le rapport)
- [ ] Aucune conclusion basee sur une seule etude
- [ ] Risques et effets secondaires identifies (meme si l'affirmation ne porte que sur les benefices)
- [ ] Si aucune preuve solide trouvee → verdict = "PREMATURE" ou "NON PROUVE"

Si un point echoue, relancer des recherches ciblees avant de passer a la synthese.

## 6. Phase SYNTHESE — ultrathink

Analyser l'ensemble des preuves avec un raisonnement approfondi, en pesant les preuves contradictoires.

Generer le rapport selon le template dans [REPORT_TEMPLATE.md](REPORT_TEMPLATE.md).
Utiliser la grille de niveaux de preuve dans [EVIDENCE_HIERARCHY.md](EVIDENCE_HIERARCHY.md).

Le rapport final est toujours en francais.

## Regles

- **Minimum 3 sources independantes** avant toute conclusion
- **Ne jamais conclure a partir d'une seule etude** — exiger la replication
- **Signaler les mots weasel** : "pourrait", "suggere", "potentiel" = pas prouve
- **Privilegier les meta-analyses** sur les etudes isolees
- **Mentionner les limites** : taille echantillon, duree, population etudiee
- **Langue des recherches** : anglais pour les bases scientifiques, francais pour ANSES/EFSA
- **Graceful degradation** : si un outil MCP ou une base n'est pas disponible, continuer avec les autres sources
