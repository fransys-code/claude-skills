---
name: memory-reorganize
description: "Audits and reorganizes Claude Code's auto-memory (~/.claude/projects/<workspace>/memory/) according to the system's best practices: 4 strict types (user/feedback/project/reference), Why/How to apply format, MEMORY.md as a pure index, absolute dates, no duplicates, no content derivable from filesystem or git. Detects violations, proposes a reorganization plan, applies after validation. Use this skill whenever the user wants to clean, reorganize, audit, or refactor their memory, or when they say their memory is messy or disorganized."
user-invocable: true
argument-hint: "[optional: focus on a specific aspect e.g. 'duplicates', 'frontmatter', 'index']"
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
  - Grep
  - AskUserQuestion
---

# Memory Reorganize

Audits Claude Code's auto-memory and reorganizes it according to best practices defined in the system prompt. Detects violations, proposes a plan, applies after user validation.

**Target directory**: `~/.claude/projects/<workspace>/memory/`

The `<workspace>` is the encoded identifier of the working directory (e.g. for `~/.claude/skills`, it will be `-home-user--claude-skills`). List `~/.claude/projects/` to identify the right folder at invocation time.

## Reference files (load on demand)

- **[BEST_PRACTICES.md](BEST_PRACTICES.md)** — full rules: 4 types, frontmatter, Why/How body, MEMORY.md = index, exclusions
- **[EXAMPLES.md](EXAMPLES.md)** — concrete before/after refactors and anti-pattern table

Read `BEST_PRACTICES.md` BEFORE phase 2 (audit). Read `EXAMPLES.md` when entering phase 8 (refactor plan) to calibrate style.

## Progression

Copy and follow this checklist:

```
- [ ] Phase 1: Inventory (list files, read MEMORY.md)
- [ ] Phase 2: Per-file audit of each memory file (vs BEST_PRACTICES.md)
- [ ] Phase 3: MEMORY.md audit (index format)
- [ ] Phase 4: Detect duplicates and overlaps
- [ ] Phase 5: Detect derivable content (filesystem, git, CLAUDE.md)
- [ ] Phase 6: Freshness check (broken refs, relative dates)
- [ ] Phase 7: Violation report to the user
- [ ] Phase 8: Reorganization plan (consult EXAMPLES.md)
- [ ] Phase 9: User validation (AskUserQuestion)
- [ ] Phase 10: Backup + apply + final verification
```

If `$ARGUMENTS` contains a focus (e.g. "duplicates", "frontmatter"), prioritize the relevant phases but still run the inventory (phase 1-2).

## Phase 1: Inventory

```bash
# Identify the active memory workspace
ls ~/.claude/projects/

# Then on the right folder:
ls -la "~/.claude/projects/<workspace>/memory/"
wc -l "~/.claude/projects/<workspace>/memory/"*.md
```

Note:
- Number of memory files
- MEMORY.md size (lines)
- Orphans (file present but absent from MEMORY.md)
- Broken refs (link in MEMORY.md to missing file)

## Phase 2: Per-file audit

For each file (other than MEMORY.md), check against `BEST_PRACTICES.md`:

1. Frontmatter present and complete (`name`, `description`, `type`)
2. `type` in {user, feedback, project, reference}
3. If type=feedback or type=project: `**Why:**` and `**How to apply:**` lines present
4. Specific description, not generic
5. Absolute dates (no "recently", "last week")
6. No forbidden content (derivable code/architecture/git)
7. Type ↔ content consistency

Keep a violations table per file.

## Phase 3: MEMORY.md audit

1. No frontmatter
2. Each entry line < 150 characters
3. Format `- [Title](file.md) — hook` respected
4. No memory content written directly (only links)
5. Total < 200 lines
6. Semantic sections by topic, not chronological
7. Each entry points to an existing file

## Phase 4: Duplicates and overlaps

Detect:
- Two files covering the same topic
- Information appearing in multiple files
- Memory that should be merged with another

Use Grep to find common keywords across files.

## Phase 5: Derivable content

For each memory, apply the "where to find the info" test (see BEST_PRACTICES.md > Reference vs derivable). If derivable from filesystem / git / CLAUDE.md → candidate for deletion or reduction.

## Phase 6: Freshness

- Memories referencing paths/files: verify they still exist (Bash `ls`)
- Memories referencing MCP/env vars/services: verify current configuration
- Memories dated > 6 months: flag for manual review

## Phase 7: Violation report

Format:

```markdown
## Memory audit — [DATE]
**Current state**: N files, MEMORY.md = X lines

### Critical violations
- [File] — Missing frontmatter / invalid type / line >150 chars

### Medium violations
- [File] — type=feedback without Why/How / relative date

### Derivable content to remove
- MEMORY.md L8-19 — "Existing skills" derivable from `ls`

### Duplicates
- fileA.md and fileB.md cover topic X

### Broken refs / Orphans
- MEMORY.md L42 → nonexistent_file.md
- foo.md present but absent from MEMORY.md
```

## Phase 8: Reorganization plan

Before proposing, consult `EXAMPLES.md` to calibrate refactor style.

Structured plan:
1. **Files to delete**: list + reason
2. **Files to merge**: A+B → C with justification
3. **Files to refactor**: add frontmatter / Why / How / convert dates
4. **MEMORY.md**: rewritten version with semantic sections

Show a conceptual before/after diff for MEMORY.md.

## Phase 9: User validation

Use `AskUserQuestion` with options:
- "Apply the full plan"
- "Apply only critical violations"
- "Show me detail of one change before deciding"
- "Cancel"

NEVER apply without explicit validation.

## Phase 10: Application

1. **Backup**: copy each modified file to `*.backup-YYYY-MM-DD` BEFORE modification
2. Delete identified files (`rm`)
3. Merge (read sources, write target, delete sources)
4. Refactor (Edit to add frontmatter, Why/How, absolute dates)
5. Rewrite MEMORY.md with semantic sections + entries < 150 chars
6. **Final verification**: re-read MEMORY.md and each modified file

Report at end:
- N files deleted / merged / refactored
- New MEMORY.md size
- Backup paths

## Golden rules

- **Never invent** a memory that doesn't exist. Reorganize, don't create from scratch.
- **Preserve valuable content**: if a memory violates rules but contains useful info, refactor rather than delete.
- **Backup before everything**: `*.backup-YYYY-MM-DD` before any modification.
- **Ask when in doubt**: ambiguous memory → AskUserQuestion rather than deciding alone.
- **Don't touch files outside the memory directory**: no modification to CLAUDE.md, settings.json, skills.
