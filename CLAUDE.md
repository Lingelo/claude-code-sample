# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Claude Code Configuration Repository** - a shareable collection of custom agents, slash commands, scripts, and settings to enhance Claude Code workflows. The configuration can be distributed as a plugin via the marketplace system or used at the project/global level.

## Repository Architecture

### Configuration Structure

```
.claude/
├── agents/          # Custom agents for specialized tasks
├── commands/        # Slash commands for workflows
├── scripts/         # Utility scripts (statusline, hooks, etc.)
├── settings.json    # Shared team configuration (versioned)
└── settings.local.json  # Personal overrides (gitignored)

marketplace/
├── .claude-plugin/
│   └── marketplace.json           # Plugin marketplace configuration
└── claude-code-config/            # lingelo-base plugin package
    ├── agents/                    # explore-code agent
    ├── commands/                  # /epct command
    ├── scripts/                   # statusline script
    └── settings.json              # Shared settings
```

**Key principles:**
- `settings.json` is shared via Git for team-wide standards
- `settings.local.json` is personal and never committed
- `marketplace/` contains distributable plugin packages

### Plugin Marketplace

**lingelo-tools** (`marketplace/.claude-plugin/marketplace.json`)
- Plugin marketplace configuration for distributing Claude Code configurations
- Contains the `lingelo-base` plugin with all custom configurations
- Can be installed via `/plugin` command in any Claude Code session
- Enables one-command installation of all custom agents, commands, and scripts

**lingelo-base plugin** (`marketplace/claude-code-config/`)
- Packaged collection of all custom configurations
- Includes explore-code agent, /epct command, statusline script
- Distributable as a single plugin unit
- Version: 1.0.0

### Custom Agents

**explore-code** (`.claude/agents/explore-code.md` and `marketplace/claude-code-config/agents/explore-code.md`)
- Specialized agent for codebase exploration
- Automatically invoked by `/epct` during EXPLORE phase
- Runs in isolated context to preserve main conversation
- Discovers patterns, conventions, dependencies, and examples
- Reports findings in structured format

### Custom Commands

**/epct** (`.claude/commands/epct.md` and `marketplace/claude-code-config/commands/epct.md`)
- Systematic implementation workflow: **Explore → Plan → Code → Test**
- **EXPLORE**: Launch parallel agents to find relevant code/patterns
- **PLAN**: Create detailed implementation strategy, ask for clarification
- **CODE**: Implement following discovered patterns, stay strictly in scope
- **TEST**: Run relevant tests only (not entire suite), verify changes work

**Critical rules for /epct:**
- Use parallel agents during EXPLORE phase
- Never exceed task boundaries
- Match existing codebase style and patterns
- Run only tests related to changes
- If tests fail, return to PLAN phase

### Scripts

**statusline-ccusage.sh** (`.claude/scripts/statusline-ccusage.sh`)
- Custom statusline displaying git status, costs, tokens, model info
- Requires: `ccusage` (v15.9.4+), `jq`, `git`
- Shows only conversation tokens (input + output), excludes cache tokens
- Configured in settings.json with relative path: `./.claude/scripts/statusline-ccusage.sh`

## Configuration Management

### Installation Methods

**Method 1: Plugin Marketplace (Recommended)**
```bash
# Install from local marketplace
/plugin

# Or add as remote marketplace on GitHub
/plugin marketplace add Lingelo/claude-code-sample

# Then install lingelo-base plugin
/plugin
```

**Method 2: Manual Global Deployment**
```bash
cp -r .claude/agents/* ~/.claude/agents/
cp -r .claude/commands/* ~/.claude/commands/
cp -r .claude/scripts/* ~/.claude/scripts/
```

**Method 3: Per-Project**
Keep `.claude/` in the project (already done here)

**Hybrid approach:**
- Generic agents/commands in `~/.claude/`
- Project-specific settings in `./.claude/`
- Claude Code automatically merges both locations

### Permissions

**Allowed:**
- WebSearch
- git commit/add (automated version control)
- npm/yarn (package management)
- mcp__context7 (documentation MCP)

**Denied:**
- Read(.env*) - All environment files protected

## Development Workflow

### For adding new configurations to this repo:

1. Test changes locally in `.claude/`
2. Update plugin in `marketplace/claude-code-config/` to match
3. If adding to shared config, update `.claude/settings.json`
4. If personal preference, use `.claude/settings.local.json` (not committed)
5. Update README.md and CLAUDE.md if adding new agents/commands
6. Update version in `marketplace/.claude-plugin/marketplace.json` if needed
7. Commit with descriptive message

### For distributing via marketplace:

1. Ensure `marketplace/claude-code-config/` is up to date
2. Test plugin installation locally with `/plugin`
3. Update version number in marketplace.json
4. Push to GitHub
5. Users can add marketplace: `/plugin marketplace add Lingelo/claude-code-sample`

### When using /epct for feature development:

1. Run `/epct [feature description]`
2. Let explore-code agent discover relevant patterns
3. Review the PLAN phase before proceeding
4. Implementation will follow discovered conventions
5. Only run tests related to your changes

## Important Notes

- This repo has no build/test commands - it's pure configuration
- settings.local.json is gitignored by design
- Statusline script path is relative for portability
- Agent/command files use frontmatter for metadata (name, description, color, model)
- Marketplace structure in `marketplace/` for plugin distribution
- Keep `.claude/` and `marketplace/claude-code-config/` in sync
