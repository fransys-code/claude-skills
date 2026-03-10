# Template du rapport — Fact Check

Generer le rapport final en suivant exactement cette structure :

```markdown
# Fact Check

**Affirmation :** [affirmation ou titre de l'article]
**Source :** [media/auteur] — Fiabilite : [Tier X]
**Date :** [date de verification]
**Verdict global :** [selon VERDICT_SCALE.md]
**Confiance :** [ELEVE / MOYEN / FAIBLE]

---

## Resume

[2-3 phrases : ce qui est vrai, ce qui est exagere/faux, et pourquoi]

## Decomposition des affirmations

| # | Sous-affirmation | Verdict | Confiance | Sources |
|---|-----------------|---------|-----------|---------|
| 1 | [fait specifique] | [verdict] | [niveau] | [refs] |
| 2 | [chiffre cite] | [verdict] | [niveau] | [refs] |
| 3 | [attribution/citation] | [verdict] | [niveau] | [refs] |

## Contexte manquant

[Elements importants omis par l'article original qui changent la perception]

## Biais detectes

| Biais | Detail |
|-------|--------|
| [type de biais] | [explication concrete] |

## Ce que disent les sources primaires

| Source primaire | Ce qu'elle dit reellement | Ecart avec l'article |
|----------------|--------------------------|---------------------|
| [etude/rapport/communique] | [citation ou resume] | [deformation eventuelle] |

## Positions officielles (si applicable)

| Organisme | Position | Date |
|-----------|----------|------|
| [ex: CNRS] | [position] | [date] |

## Sources consultees

| # | Source | Type | Fiabilite | URL |
|---|--------|------|-----------|-----|
| 1 | [nom] | [type] | [Tier X] | [url] |
| 2 | [nom] | [type] | [Tier X] | [url] |
```

## Regles du template

- Chaque sous-affirmation a son propre verdict independant
- Toujours inclure le contexte manquant, meme s'il n'invalide pas l'article
- Les biais sont factuels, pas des jugements de valeur
- Citer les sources primaires directement, pas les reprises
- Ne pas laisser de cellule vide — ecrire "Non trouve" si l'information manque
- Le verdict global est la synthese ponderee des sous-verdicts
- Signaler explicitement le decalage titre/contenu si present
