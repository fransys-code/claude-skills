# API Integration — Fact Check

## 1. Google Fact Check API

Queries the database of existing fact-checks (Snopes, PolitiFact, AFP Factuel, Les Decodeurs, etc.).

### Call

```bash
# French search
curl -s "https://factchecktools.googleapis.com/v1alpha1/claims:search?query=QUERY_ENCODED&languageCode=fr&key=${GOOGLE_FACTCHECK_API_KEY}" | python3 -m json.tool

# English search
curl -s "https://factchecktools.googleapis.com/v1alpha1/claims:search?query=QUERY_ENCODED&languageCode=en&key=${GOOGLE_FACTCHECK_API_KEY}" | python3 -m json.tool
```

### Useful Response

- `claims[].text` — The verified claim
- `claims[].claimant` — Who made the claim
- `claims[].claimReview[].publisher.name` — Who fact-checked (ex: AFP Factuel)
- `claims[].claimReview[].textualRating` — The verdict (ex: "False", "Partly true")
- `claims[].claimReview[].url` — Link to full fact-check

### API Key

Free, obtain at: https://console.cloud.google.com/apis/credentials
Enable "Fact Check Tools API" in the Google Cloud console.

Environment variable: `GOOGLE_FACTCHECK_API_KEY`

---

## Required Environment Variables

Add to `~/.config/fish/config.fish`:

```fish
set -gx GOOGLE_FACTCHECK_API_KEY "your_key_here"
```
