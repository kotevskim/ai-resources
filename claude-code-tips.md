# Claude Code — Tips

---

## Commands

| Command | What it does |
|---|---|
| `/clear` | Wipes conversation history + resets token usage. Session stays open, working directory and CLAUDE.md survive. |
| `/compact` | Compresses history instead of wiping. Pass hints: `/compact retain the error handling patterns`. |
| `/exit` or `Ctrl+D` | Ends the session entirely, returns to terminal. |
| `/fast` | Makes Claude Opus 4.6 respond ~2.5x faster at higher token cost. Toggle on/off. Combine with `/effort low` for max speed on simple tasks. Requires v2.1.36+. |

**When to use `/clear` vs `/compact` vs `/exit`:**
- `/clear` → new **unrelated** task, want a fresh context but keep CLAUDE.md memory
- `/compact` → context full **mid-task**, or starting a **related** task and want some carry-over
- `/exit` → done coding for now

---

## Effort Levels

Control how hard Claude thinks and how many tokens it uses:

```
/effort low | medium | high | max | auto
```

| Level | Use when |
|---|---|
| `low` | Quick lookups, renaming, grep-style questions (~⅓ the tokens of high) |
| `medium` | Default. Everyday feature work, code reviews, routine tasks |
| `high` | Complex logic, tricky queries, architecture decisions |
| `max` | Genuinely stuck on a hard problem. Opus only. Burns tokens fast. |

> Per-turn override: add `ultrathink` to your prompt to temporarily boost to High for a single response.

---

## Referencing Files with `@`

- `@path/to/file.kt` — adds the file to context without Claude having to find and read it
- `@file.kt#L76-82` — reference specific line ranges to keep context tight
- Multiple files: `@TransactionsRepo.kt and @Repo.kt`
- In VS Code: select lines → `Option+K` (Mac) / `Alt+K` (Windows/Linux) to insert a line-range reference
- Claude also auto-loads any CLAUDE.md files in the referenced file's directory

---

## Claude Configuration Files

| File | Who writes it | Commit? | Purpose |
|---|---|---|---|
| `CLAUDE.md` (project root) | You | ✅ Yes | Project structure, stack, conventions, commands |
| `.claude/rules/*.md` | You | ✅ Yes | Task-specific rules loaded only when file pattern matches |
| `CLAUDE.local.md` | You | ❌ No | Personal preferences, not shared with team |
| `~/.claude/CLAUDE.md` | You | ❌ No | Global preferences across all projects |
| `MEMORY.md` | Claude auto | ❌ No | Auto-accumulated learnings in `~/.claude/projects/` |

**Rule of thumb:**
- Keep CLAUDE.md under ~500 lines
- Keep in CLAUDE.md: project structure, stack, conventions, build commands — things needed every session
- Move to `.claude/rules/`: specific conventions that only apply to a subset of files (e.g. JOOQ rules targeting `**/data/**/*.kt`, serialization rules targeting `**/api/**/*.kt`)

---

## Plugins

Plugins are installable packages that extend Claude Code with new capabilities — they bundle skills, subagents, slash commands, and MCP server configs into a single installable unit. Install via `/plugin marketplace add org/repo-name`.

Auto-invocation is **unreliable in practice** — always mention plugins explicitly for anything important.

### context7

Fetches up-to-date, version-specific documentation directly into your context. Prevents Claude from using outdated API knowledge — especially valuable for fast-moving frameworks like Flutter and Ktor.

Always invoke explicitly:
```
"Use context7 to check the current Ktor ContentNegotiation API"
"Use context7 for the latest Flutter Navigator 2.0 docs"
```

Install:
```bash
claude mcp add context7 --scope user
```

---

## MCP

MCP (Model Context Protocol) servers give Claude access to external tools and data sources — databases, GitHub, browser automation, and more. Configured per scope:

| Scope | Where stored | Shared with team? |
|---|---|---|
| `project` | `.mcp.json` in project root | ✅ Yes (commit to git) |
| `user` | `~/.claude.json` | ❌ No (all your projects) |
| `local` | `~/.claude.json` under project path | ❌ No (this project only) |

Typical pattern: commit `.mcp.json` with shared servers (GitHub, project DB). Each dev adds their own auth tokens via local/user scope.

---

## Not Using (and Why)

### Slash Commands (`.claude/commands/`)

Markdown files where the filename becomes the command (e.g. `db-migrate.md` → `/db-migrate`). Meant for repeatable multi-step procedures Claude executes on demand.

**Why not using:** Any sequential CLI workflow is better handled by a shell script. And for anything that needs Claude's judgment, you can just reference the script inline: `"run @scripts/db-migrate.sh and interpret any errors"`. A slash command is only worth adding if you find yourself typing the same prompt 10+ times — otherwise it's just an extra file to maintain.

### Skills (`.claude/skills/`)

Reusable instruction sets that Claude is supposed to auto-load when it detects a match — essentially giving Claude a playbook for a specific type of task (e.g. "how to write JOOQ queries in this project").

**Why not using:** For a solo side project, CLAUDE.md and `.claude/rules/` already cover this. Skills add value when you're packaging reusable capabilities across multiple projects or sharing them with a team. The auto-invocation is also unreliable — you'd end up explicitly invoking them anyway, at which point a well-written CLAUDE.md section does the same job with less overhead.