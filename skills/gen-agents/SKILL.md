---
name: gen-agents
description: Use when the user wants to create, update, or review an AGENTS.md context file for their codebase, or when starting a new project that needs agent instructions
---

# Generate AGENTS.md

## Core Principles

1. **Default is empty.** Only include information whose absence causes agent failure.
2. **Detect, don't generate.** Extract facts from config/CI files. Never fabricate content.
3. **Ask, don't assume.** Use AskUserQuestion for what files cannot reveal.
4. **Format, don't narrate.** Bullet points only. No prose, no rationale, no architecture descriptions.
5. **Differential Context.** `Include = (project practice - language/framework default) + prohibitions`

## Workflow

### Phase 1: Scan (automatic)

Detect build/test/lint commands from these sources (priority order):
1. `.github/workflows/*.yml` / `.gitlab-ci.yml` — CI is the source of truth
2. `Makefile` / `justfile` — task runner targets
3. `package.json` scripts / `pyproject.toml` [tool.*] / `Cargo.toml`
4. Linter config existence: `.eslintrc`, `ruff.toml`, `.prettierrc`, `biome.json`

Only extract: command strings, tool names, runtime versions. Do NOT read source files.

### Phase 2: Ask (3 questions via AskUserQuestion)

**Q1 (required):** Present detected commands for confirmation.
"I detected these commands. Please confirm or correct:"
- Test: `{detected}`
- Lint: `{detected}`
- Build: `{detected}`

**Q2 (required):** "Are there files or directories that must never be modified? (e.g., generated code, vendored deps, contract files)"

**Q3 (required):** "Are there project practices that deviate from language/framework defaults? Only things agents would likely get wrong. (e.g., 'use Zustand not Redux', 'Result type for errors — no throw', 'Repository pattern only — no direct ORM in handlers'). Answer 'none' if not applicable."

### Phase 3: Generate

Combine scan results + user answers. Rules:
- Only `Commands` section is always present (if any commands exist)
- `Constraints` and `Conventions` sections appear only if user provided content
- No content beyond what was detected or user-confirmed
- Target: 100-250 words. Hard limit: 300 words.

## Output Template

```markdown
# AGENTS.md

## Commands
- Test: `{command}`
- Lint: `{command}`
- Build: `{command}`
- Type check: `{command}`

## Constraints
- {protected_file_or_dir_with_reason}

## Conventions
- {deviation_from_framework_defaults}
```

## Anti-patterns

| Never do this | Why |
|---|---|
| List directory structure | Agents discover this via filesystem tools |
| Describe architecture | Increases exploration, reduces success rate |
| Add style guidelines | Linters already enforce this |
| Summarize README/docs | Duplicates existing information |
| Auto-generate without user confirmation | LLM-generated context is harmful |
| Exceed 300 words | Verbosity degrades agent performance |
| Include "nice to have" info | If removal doesn't cause failure, don't include it |
