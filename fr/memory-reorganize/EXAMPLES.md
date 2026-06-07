# Exemples avant/apres

Cas types de refactorisation de memoires. A consulter pour calibrer le style et l'agressivite des suppressions.

## Exemple 1 : memoire `project` sans Why/How (frequent)

### AVANT (52 lignes — fichier `data-pipeline.md`)

```markdown
---
name: data-pipeline architecture
description: Structure et catalogue de la skill data-pipeline
type: project
---
# data-pipeline — extension 2026-04-15

Skill ETL avec 4 fichiers de support :
- `SKILL.md` — orchestration, 6 phases
- `PHASES.md` — workflow detaille phases 1-6
- `CONNECTORS.md` — catalogue 14 connecteurs (Postgres, BigQuery, S3...)
- `TRANSFORMS.md` — 38 transformations en 5 categories A-E

## Categories transformations
- A. Cleaning (#1-#8)
- B. Joining (#9-#15)
- C. Aggregation (#16-#22)
[... 25 lignes de catalogue ...]

## Connecteurs ajoutes 2026-04-15
- **Cloud** : Snowflake, Databricks, Redshift
[... 18 lignes de connecteurs ...]
```

### Problemes
- Pas de `**Why:**` ni de `**How to apply:**` (obligatoire pour type=project)
- Le catalogue transformations/connecteurs duplique des fichiers existants dans la skill (`TRANSFORMS.md`, `CONNECTORS.md`)
- Reglement operationnel "secrets via vault" : valeur reelle, mais noyee dans 30 lignes derivables

### APRES (20 lignes)

```markdown
---
name: data-pipeline skill
description: Skill ETL pour orchestrer ingestions multi-sources avec validation au runtime. Couvre Postgres, BigQuery, S3, Snowflake.
type: project
---

Skill d'orchestration ETL avec validation forte au runtime, alignee best practices 2026.

**Why:** Standardiser les pipelines internes apres 3 incidents en Q1 lies a des schemas incoherents en sortie. La validation au runtime est non-negociable depuis le post-mortem du 2026-02-14.

**How to apply:** Invoquer la skill pour toute creation/modification de pipeline d'ingestion. Catalogue connecteurs/transformations dans les fichiers support — ne PAS dupliquer ici.

## Regles operationnelles non derivables
- Secrets : toujours via Vault, jamais en variable d'env
- Validation au runtime obligatoire (post-mortem 2026-02-14)
- Pas de DROP TABLE sans dry-run prealable
- Schedule : cron via Airflow uniquement, pas de cron Linux brut
```

### Lecon
- Garder ce qui est **operationnel et non derivable** (regles imposees suite a un incident, contraintes legales)
- Supprimer ce qui est **derivable des fichiers de la skill** (catalogues, listes de connecteurs)
- Ajouter Why/How meme retroactivement — la motivation doit etre reconstituable

---

## Exemple 2 : MEMORY.md = chaos → index pur

### AVANT (44 lignes, contenu inline)

```markdown
# Memory

## Skills directory structure
- Skills are stored in `~/.claude/skills/`
- Each skill has a `SKILL.md` with frontmatter

## Existing skills
- `data-pipeline` — ETL orchestration
- `incident-response` — runbook structure
- `release-checklist` — pre-deploy validation
[... 11 lignes de listing ...]

## Skills rewrite (2026-03-10)
- Both `data-pipeline` and `release-checklist` rewritten to match standards
- Now use: checklist progression, parallel agents, ultrathink
- Key updates: validation runtime, schemas deprecated, KPIs mesurables [... 230 chars sur une ligne ...]

## release-checklist architecture
- `SKILL.md` — Main orchestration: pre-check + 4 parallel validations
- `STAGES.md` — 8 deploy stages
[... 6 lignes inline ...]
```

### Problemes
- Pas un index : sections multi-paragraphes
- "Skills directory structure" et "Existing skills" entierement derivables d'un `ls`
- "Skills rewrite (2026-03-10)" = log de travail (qui-a-fait-quoi), pas une memoire
- "release-checklist architecture" = contenu inline qui devrait etre un fichier dedie OU etre supprime (derivable des fichiers de la skill)
- Lignes >150 chars

### APRES (5 lignes)

```markdown
# Memory

## Infrastructure
- [Data pipeline](data-pipeline.md) — ETL multi-sources avec validation runtime imposee post-incident Q1
- [Incident response](incident-response.md) — runbook escalade L1→L3, contraintes SLA et notification stakeholders
- [Release checklist](release-checklist.md) — validation pre-deploy, schedules figes par equipe SRE
```

### Lecon
- MEMORY.md doit etre un menu : pointeurs vers les fichiers, jamais le contenu
- Supprimer agressivement les sections qui sont des logs ou des resumes derivables
- Sections semantiques par theme ("Infrastructure", "Onboarding", "Clients"), pas chronologiques
- Une ligne par memoire, sous 150 chars, format strict `- [Title](file.md) — hook`

---

## Exemple 3 : memoire bien structuree mais trop bavarde

### AVANT (26 lignes — fichier `release-checklist.md`)

```markdown
---
name: release-checklist skill architecture
description: Architecture of the release-checklist skill (refactored 2026-04-14)
type: project
originSessionId: 00000000-0000-0000-0000-000000000000
---
Pre-deploy validation skill for backend services.
Repo racine: `~/projects/internal-platform/`

**Why:** Team wanted the skill aligned with 2026 best practices [...]
**How to apply:** When working on this skill or its outputs [...]

Architecture (after 2026-04-14 refactor):
- `SKILL.md` (118 lines) — Lean orchestrator
- `STAGES.md` — 8 deploy stages with gating criteria
- `ROLLBACK.md` — 4 rollback procedures
- `KPIS.md` — measurable thresholds per stage
- `INCIDENTS.md` — past incidents to avoid
- `SLA_GRID.md` — SLA per service tier

Key rules:
- No deploy on Friday after 16:00 (SRE policy)
- Mandatory canary 5% before full rollout
- Rollback within 15min if error rate > 1%
- Postmortem required for any P1/P2
```

### Problemes
- Frontmatter contient `originSessionId` (non standard)
- `description` redondante (info dans `Why`)
- Section "Architecture" entierement derivable d'un `ls` du dossier de la skill
- "Key rules" : valeur reelle (regles operationnelles non derivables, imposees par SRE) — a garder

### APRES (16 lignes)

```markdown
---
name: release-checklist skill
description: Skill de validation pre-deploy pour services backend. Bloque les deploys risques et impose les checks obligatoires SRE.
type: project
---

Skill pre-deploy pour services backend internes. Repo racine : `~/projects/internal-platform/`.

**Why:** Reduire les rollbacks (12% des deploys avant introduction). Imposer la canary, le no-deploy-Friday et le rollback automatique <15min sur erreur >1%, suite aux 3 incidents P1 de Q4 2025.

**How to apply:** Invoquer pour toute preparation de release / deploy. Architecture detaillee dans les fichiers support — ne PAS dupliquer ici.

## Regles operationnelles non derivables
- **No deploy Friday > 16h** (politique SRE)
- **Canary 5% obligatoire** avant rollout complet
- **Rollback < 15min** si error rate > 1%
- **Postmortem requis** pour tout P1/P2
```

### Lecon
- Les `Key rules` operationnelles meritent leur propre section nommee : ce sont des contraintes non derivables
- L'architecture des fichiers support n'a pas sa place : le `ls` la donne
- Description courte mais specifique : "skill de X" plutot que "Architecture de X"
- Supprimer `originSessionId` (non standard, pas utile)

---

## Anti-patterns frequents a surveiller

| Anti-pattern | Indice |
|---|---|
| Memoire = log de session | Liste de "qui a fait quoi", dates de refactor sans Why |
| Memoire = catalogue derivable | Reproduit la structure d'un dossier/skill |
| Memoire = doublon CLAUDE.md | Reglements globaux deja dans CLAUDE.md |
| Memoire `project` sans Why/How | Faits sans motivation ni contexte d'application |
| MEMORY.md = contenu inline | Sections multi-paragraphes au lieu de pointeurs |
| Frontmatter exotique | `originSessionId`, `tags`, `priority` non standards |
| Dates relatives | "recemment", "la semaine derniere", "il y a 2 jours" |
