# init-context

Generate minimal, effective AGENTS.md files for Claude Code.

Based on research ([arxiv 2602.11988](https://arxiv.org/abs/2602.11988)) showing that verbose context files reduce agent performance. Uses the **Differential Context** principle: include only what differs from defaults + what is prohibited.

## Installation

Run two commands in Claude Code:

```bash
# 1. Add the marketplace
/plugin marketplace add tmdgusya/init-context-skill

# 2. Install the plugin
/plugin install init-context@tmdgusya-init-context-skill
```

To update:

```bash
/plugin update init-context
```

## Usage

In Claude Code, invoke the skill:

```
/init-context:init
```

The skill will:
1. **Scan** your config and CI files for build/test/lint commands
2. **Ask** 3 targeted questions to capture what code alone can't reveal
3. **Generate** a concise AGENTS.md (100-250 words, hard limit 300)

## What goes in, what stays out

| Included | Excluded |
|----------|----------|
| Build/test/lint commands | Directory structure |
| Protected files/dirs | Architecture descriptions |
| Deviations from framework defaults | Style guides (linters handle this) |
| | README/docs duplication |

## License

MIT
