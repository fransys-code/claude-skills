# Claude Code auto-memory best practices

Exhaustive reference of the rules this skill enforces. Sourced from Claude Code's internal system prompt (the "auto memory" section).

## The 4 memory types (strict)

| Type | Content | When to save |
|---|---|---|
| `user` | Role, preferences, expertise, personal context of the user | We learn a detail about the user |
| `feedback` | Behavior rule (correction OR validation) | The user corrects or validates an approach |
| `project` | Decision, deadline, motivation behind a piece of work | We learn a non-trivial reason about a project |
| `reference` | Pointer to an external system (Linear, Grafana, MCP path...) | We learn where to find info outside the repo |

Any file that doesn't fit into one of these 4 types is suspect.

## Required frontmatter

```markdown
---
name: {{short name}}
description: {{one line, specific, used to judge future relevance}}
type: {{user|feedback|project|reference}}
---
```

`originSessionId` field: tolerated but non-standard, can be removed.

## Body structure (feedback AND project only)

Must contain:
- The rule or fact first
- A `**Why:**` line (motivation, incident, constraint)
- A `**How to apply:**` line (when/where this rule applies)

**Without these two lines, it's impossible to judge edge cases later.** This is what turns a note into an actionable rule.

For `user` and `reference` types, these lines are not required (the info is descriptive, not behavioral).

## MEMORY.md = pure index

Strict rules:
- No frontmatter
- One line per memory, format: `- [Title](file.md) — one-line hook`
- Each line < 150 characters
- Total < 200 lines (beyond = truncated by the harness)
- No memory content written directly (only pointers)
- Semantic organization by topic, not chronological

Allowed sections: `## Theme` followed by a list of pointers. No multi-paragraph sections.

## What you MUST NOT save

| Category | Example |
|---|---|
| Code patterns, architecture, paths | "Component X is in src/components/" |
| Git history, who-changed-what | "Refactor on 2026-04-14" without why |
| Debug recipes or fixes | "Bug X is fixed with Y" |
| CLAUDE.md duplicates | Already in global or local CLAUDE.md |
| Ephemeral state | In-progress tasks, current conversation context |
| Lists derivable from `ls` | "Existing skills: a, b, c..." |
| Filesystem summaries | A skill's architecture (the files exist in the skill) |

These exclusions apply **even if the user explicitly asked to save**. In that case, ask what was *surprising* or *non-obvious* — that's what deserves memory.

## Dates

Always absolute (`2026-04-28`), never relative (`yesterday`, `last week`, `recently`).

Automatic conversion at initial save time, never retroactively (otherwise loss of info).

## Reference vs derivable

The "where to find the info" test:
- Info findable via `ls` or local file read → derivable, not memory
- Info findable via `git log` or `git blame` → derivable, not memory
- Info findable in CLAUDE.md → already in context, not memory
- Info findable nowhere in the repo → memory candidate

Edge case: technical info stored on a remote VPS/server → legitimate memory (the user isn't always connected to the VPS).

## Lifecycle

- **Create**: only when the info is non-derivable, durable, actionable
- **Update**: check that no existing memory already covers the topic before creating a new one
- **Delete**: when info becomes obsolete OR has become derivable (e.g. added to CLAUDE.md)
- **Verify**: before citing a memory in a recommendation, confirm paths/refs still exist
