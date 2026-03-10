# API Integration — Fact Check

## 1. Google Fact Check API

Interroge la base de donnees des fact-checks existants (Snopes, PolitiFact, AFP Factuel, Les Decodeurs, etc.).

### Appel

```bash
# Recherche en francais
curl -s "https://factchecktools.googleapis.com/v1alpha1/claims:search?query=QUERY_ENCODED&languageCode=fr&key=${GOOGLE_FACTCHECK_API_KEY}" | python3 -m json.tool

# Recherche en anglais
curl -s "https://factchecktools.googleapis.com/v1alpha1/claims:search?query=QUERY_ENCODED&languageCode=en&key=${GOOGLE_FACTCHECK_API_KEY}" | python3 -m json.tool
```

### Reponse utile

- `claims[].text` — L'affirmation verifiee
- `claims[].claimant` — Qui a fait l'affirmation
- `claims[].claimReview[].publisher.name` — Qui a fact-checke (ex: AFP Factuel)
- `claims[].claimReview[].textualRating` — Le verdict (ex: "Faux", "Partiellement vrai")
- `claims[].claimReview[].url` — Lien vers le fact-check complet

### Cle API

Gratuite, obtenir sur : https://console.cloud.google.com/apis/credentials
Activer "Fact Check Tools API" dans la console Google Cloud.

Variable d'env : `GOOGLE_FACTCHECK_API_KEY`

---

## Variables d'environnement requises

Ajouter dans `~/.config/fish/config.fish` :

```fish
set -gx GOOGLE_FACTCHECK_API_KEY "votre_cle_ici"
```
