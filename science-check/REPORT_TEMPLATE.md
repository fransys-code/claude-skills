# Template du rapport — Science Check

Generer le rapport final en suivant exactement cette structure :

```markdown
# Verification scientifique

**Affirmation :** [affirmation originale]
**Date :** [date]
**Verdict global :** [CONFIRME / PARTIELLEMENT CONFIRME / NON PROUVE / REFUTE / PREMATURE]

---

## Consensus scientifique

[Resume en 2-3 phrases de ce que dit la science actuelle]

## Detail par sous-affirmation

| Affirmation specifique | Verdict | Niveau de preuve | Sources cles |
|------------------------|---------|------------------|--------------|
| [ex: reduit la tension] | Confirme | Meta-analyse RCT (n=2400) | [ref] |
| [ex: boost energie] | Non prouve | 1 RCT negatif (n=45) | [ref] |

## Risques et effets secondaires

| Risque | Gravite | Niveau de preuve | Population concernee |
|--------|---------|------------------|---------------------|
| [risque] | [faible/modere/grave] | [type etude] | [qui est concerne] |

## Positions officielles

| Organisme | Position | Date |
|-----------|----------|------|
| FDA | [position] | [date] |
| EFSA | [position] | [date] |
| OMS | [position] | [date] |
| ANSES | [position] | [date] |

## Red flags identifies

- [Financement par l'industrie]
- [Etudes non reproduites]
- [Tailles d'echantillon trop faibles]
- [Retractations]
- [Conflits d'interet]

## Sources consultees

| # | Source | Type | Annee | URL |
|---|--------|------|-------|-----|
| 1 | [nom] | Meta-analyse | [annee] | [url] |
| 2 | [nom] | RCT | [annee] | [url] |
| 3 | [nom] | Position reglementaire | [annee] | [url] |
```

## Regles du template

- Chaque sous-affirmation doit avoir son propre verdict independant
- Toujours inclure la taille d'echantillon (n=) quand disponible
- Ne pas laisser de cellule vide — ecrire "Non trouve" si l'information manque
- Le verdict global est la synthese ponderee des sous-verdicts (une sous-affirmation refutee peut donner "PARTIELLEMENT CONFIRME" si les autres sont confirmees)
- Les sources sont numerotees et referencees dans le tableau des sous-affirmations
