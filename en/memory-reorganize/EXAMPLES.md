# Before/after examples

Typical refactor cases. Consult to calibrate style and aggressiveness of deletions.

## Example 1: `project` memory without Why/How (frequent)

### BEFORE (52 lines — file `data-pipeline.md`)

```markdown
---
name: data-pipeline architecture
description: Structure and catalog of the data-pipeline skill
type: project
---
# data-pipeline — extension 2026-04-15

ETL skill with 4 support files:
- `SKILL.md` — orchestration, 6 phases
- `PHASES.md` — detailed workflow phases 1-6
- `CONNECTORS.md` — catalog of 14 connectors (Postgres, BigQuery, S3...)
- `TRANSFORMS.md` — 38 transformations in 5 categories A-E

## Transformation categories
- A. Cleaning (#1-#8)
- B. Joining (#9-#15)
- C. Aggregation (#16-#22)
[... 25 lines of catalog ...]

## Connectors added 2026-04-15
- **Cloud**: Snowflake, Databricks, Redshift
[... 18 lines of connectors ...]
```

### Problems
- No `**Why:**` or `**How to apply:**` (mandatory for type=project)
- The transformations/connectors catalog duplicates files already in the skill (`TRANSFORMS.md`, `CONNECTORS.md`)
- Operational rule "secrets via vault": real value, but buried in 30 lines of derivable content

### AFTER (20 lines)

```markdown
---
name: data-pipeline skill
description: ETL skill to orchestrate multi-source ingestions with runtime validation. Covers Postgres, BigQuery, S3, Snowflake.
type: project
---

ETL orchestration skill with strong runtime validation, aligned to 2026 best practices.

**Why:** Standardize internal pipelines after 3 Q1 incidents tied to inconsistent output schemas. Runtime validation is non-negotiable since the 2026-02-14 post-mortem.

**How to apply:** Invoke for any ingestion pipeline creation/modification. Connectors/transformations catalog is in support files — do NOT duplicate here.

## Non-derivable operational rules
- Secrets: always via Vault, never in env vars
- Mandatory runtime validation (post-mortem 2026-02-14)
- No DROP TABLE without prior dry-run
- Schedule: cron via Airflow only, no raw Linux cron
```

### Lesson
- Keep what is **operational and non-derivable** (rules imposed after an incident, legal constraints)
- Remove what is **derivable from the skill's files** (catalogs, connector lists)
- Add Why/How even retroactively — motivation must be reconstructable

---

## Example 2: MEMORY.md = chaos → pure index

### BEFORE (44 lines, inline content)

```markdown
# Memory

## Skills directory structure
- Skills are stored in `~/.claude/skills/`
- Each skill has a `SKILL.md` with frontmatter

## Existing skills
- `data-pipeline` — ETL orchestration
- `incident-response` — runbook structure
- `release-checklist` — pre-deploy validation
[... 11 lines of listing ...]

## Skills rewrite (2026-03-10)
- Both `data-pipeline` and `release-checklist` rewritten to match standards
- Now use: checklist progression, parallel agents, ultrathink
- Key updates: runtime validation, deprecated schemas, measurable KPIs [... 230 chars on one line ...]

## release-checklist architecture
- `SKILL.md` — Main orchestration: pre-check + 4 parallel validations
- `STAGES.md` — 8 deploy stages
[... 6 inline lines ...]
```

### Problems
- Not an index: multi-paragraph sections
- "Skills directory structure" and "Existing skills" entirely derivable from `ls`
- "Skills rewrite (2026-03-10)" = work log (who-did-what), not a memory
- "release-checklist architecture" = inline content that should be a dedicated file OR be removed (derivable from skill's files)
- Lines >150 chars

### AFTER (5 lines)

```markdown
# Memory

## Infrastructure
- [Data pipeline](data-pipeline.md) — ETL multi-source with runtime validation imposed post-Q1 incident
- [Incident response](incident-response.md) — escalation runbook L1→L3, SLA constraints and stakeholder notification
- [Release checklist](release-checklist.md) — pre-deploy validation, schedules locked by SRE team
```

### Lesson
- MEMORY.md must be a menu: pointers to files, never the content
- Aggressively remove sections that are logs or derivable summaries
- Semantic sections by theme ("Infrastructure", "Onboarding", "Clients"), not chronological
- One line per memory, under 150 chars, strict format `- [Title](file.md) — hook`

---

## Example 3: well-structured but verbose memory

### BEFORE (26 lines — file `release-checklist.md`)

```markdown
---
name: release-checklist skill architecture
description: Architecture of the release-checklist skill (refactored 2026-04-14)
type: project
originSessionId: 00000000-0000-0000-0000-000000000000
---
Pre-deploy validation skill for backend services.
Root repo: `~/projects/internal-platform/`

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

### Problems
- Frontmatter contains `originSessionId` (non-standard)
- `description` redundant (info in `Why`)
- "Architecture" section entirely derivable from `ls` of the skill folder
- "Key rules": real value (non-derivable operational rules, imposed by SRE) — to keep

### AFTER (16 lines)

```markdown
---
name: release-checklist skill
description: Pre-deploy validation skill for backend services. Blocks risky deploys and enforces mandatory SRE checks.
type: project
---

Pre-deploy skill for internal backend services. Root repo: `~/projects/internal-platform/`.

**Why:** Reduce rollbacks (12% of deploys before introduction). Enforce canary, no-deploy-Friday, and automatic rollback <15min on >1% error rate, after the 3 P1 incidents in Q4 2025.

**How to apply:** Invoke for any release/deploy preparation. Architecture details in support files — do NOT duplicate here.

## Non-derivable operational rules
- **No deploy Friday > 16:00** (SRE policy)
- **Mandatory 5% canary** before full rollout
- **Rollback < 15min** if error rate > 1%
- **Postmortem required** for any P1/P2
```

### Lesson
- Operational `Key rules` deserve their own named section: they are non-derivable constraints
- Architecture of support files has no place: `ls` provides it
- Short but specific description: "skill for X" rather than "Architecture of X"
- Remove `originSessionId` (non-standard, not useful)

---

## Frequent anti-patterns to watch for

| Anti-pattern | Indicator |
|---|---|
| Memory = session log | List of "who did what", refactor dates without Why |
| Memory = derivable catalog | Reproduces folder/skill structure |
| Memory = CLAUDE.md duplicate | Global rules already in CLAUDE.md |
| `project` memory without Why/How | Facts without motivation or application context |
| MEMORY.md = inline content | Multi-paragraph sections instead of pointers |
| Exotic frontmatter | `originSessionId`, `tags`, `priority` non-standard |
| Relative dates | "recently", "last week", "2 days ago" |
