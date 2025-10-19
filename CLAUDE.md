# CLAUDE.md

This file provides guidance to Claude Code when working with code in this repository.

---

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Repository Architecture](#repository-architecture)
- [Installation Methods](#installation-methods)
- [Core Features](#core-features)
- [Configuration Management](#configuration-management)
- [Development Workflow](#development-workflow)
- [Best Practices for Claude](#best-practices-for-claude)
- [Known Issues](#known-issues)

---

## Project Overview

This is a **Claude Code Configuration Repository** - a shareable, production-ready collection of custom agents, slash commands, scripts, and settings designed to enhance Claude Code workflows.

**Purpose:**
- Provide a systematic development workflow (EPCT methodology)
- Preserve conversation context with isolated agents
- Track costs and tokens in real-time
- Enforce security with granular permissions
- Enable team collaboration with shared configs

**Target Audience:**
- Development teams using Claude Code
- Individual developers seeking productivity tools
- DevOps engineers needing workflow automation

---

## Repository Architecture

### File Structure

```
claude-code-sample/
├── .claude/                    # Core configuration (versioned)
│   ├── agents/
│   │   └── explore-code.md    # Codebase exploration agent
│   ├── commands/
│   │   └── epct.md            # EPCT workflow command
│   ├── scripts/
│   │   └── statusline-ccusage.sh  # Cost/token tracking
│   ├── .mcp.json              # MCP servers configuration
│   ├── settings.json          # Team shared settings (Git)
│   └── settings.local.json    # Personal overrides (gitignored)
│
├── docs/                       # Detailed documentation
│   ├── GUIDE.md               # Complete configuration guide
│   ├── MCP.md                 # MCP servers documentation
│   └── VOICE.md               # Voice interaction guide
│
├── README.md                   # User-facing quick start
├── CLAUDE.md                   # This file (Claude context)
└── .gitignore                  # Excludes settings.local.json

```

### Key Architecture Principles

1. **Separation of Concerns**
   - `settings.json` : Team standards (versioned)
   - `settings.local.json` : Personal preferences (gitignored)
   - Agents/Commands/Scripts : Reusable components

2. **Configuration Layers**
   - Global (`~/.claude/`) : Available in all projects
   - Project (`./.claude/`) : Specific to this repo
   - Local (`./.claude/settings.local.json`) : Personal overrides

3. **Documentation Hierarchy**
   - `README.md` : Quick start, features, examples
   - `docs/GUIDE.md` : Technical deep-dive
   - `docs/MCP.md` : MCP servers setup
   - `CLAUDE.md` : Context for Claude Code (you are here)

---

## Installation Methods

### Method 1: Plugin Marketplace (Experimental ⚠️)

**Status:** Currently has installation issues - only commands install correctly, agents and scripts may not.

```bash
/plugin marketplace add Lingelo/lingelo-marketplace
/plugin install lingelo-base
```

**Known Issues:**
- ❌ Agents may not install to `~/.claude/agents/`
- ❌ Scripts may not install to `~/.claude/scripts/`
- ✅ Commands install correctly to `~/.claude/commands/`

**Recommendation:** Use manual installation (Method 2) until marketplace issues are resolved.

### Method 2: Manual Installation (Recommended)

**Global installation** (all projects):
```bash
git clone https://github.com/Lingelo/claude-code-sample.git
cd claude-code-sample
cp -r .claude/agents/* ~/.claude/agents/
cp -r .claude/commands/* ~/.claude/commands/
cp -r .claude/scripts/* ~/.claude/scripts/
chmod +x ~/.claude/scripts/*.sh
```

**Per-project installation** (this project only):
```bash
git clone https://github.com/Lingelo/claude-code-sample.git
cd claude-code-sample
chmod +x .claude/scripts/statusline-ccusage.sh
```

**Hybrid approach** (recommended):
- Install agents/commands globally for reuse
- Keep settings.json per-project for team sharing
- Use settings.local.json for personal preferences

---

## Core Features

### 1. EPCT Workflow (`/epct`)

**Purpose:** Systematic implementation methodology

**4 Phases:**
1. **EXPLORE** : Launch `explore-code` agent to find patterns/files
2. **PLAN** : Create detailed strategy, ask for clarification
3. **CODE** : Implement using discovered patterns
4. **TEST** : Run only relevant tests

**Critical Rules:**
- ✅ Always use parallel agents during EXPLORE
- ✅ Never exceed task scope during CODE
- ✅ Match existing codebase patterns
- ✅ Run only related tests, not entire suite
- ✅ Return to PLAN if tests fail

**Usage:**
```bash
/epct [feature description]
```

**Example:**
```bash
/epct add Redis caching for user sessions with 1-hour TTL
```

### 2. explore-code Agent

**Purpose:** Isolated codebase exploration

**Capabilities:**
- Search files and patterns exhaustively
- Analyze dependencies and imports
- Identify conventions and naming patterns
- Extract API schemas and database models
- Find relevant code examples

**Key Benefit:** Runs in isolated context, saving 60-70% tokens in main conversation

**Invocation:** Automatically launched by `/epct` during EXPLORE phase

### 3. Statusline (statusline-ccusage.sh)

**Purpose:** Real-time cost and token tracking

**Displays:**
- 🌿 Git branch and changes (+/-)
- 💰 Session cost and duration
- 📅 Daily total cost
- 🧩 Token count (input + output only, excludes cache)
- 🤖 Model name

**Requirements:**
- `ccusage` v15.9.4+ (npm install -g ccusage)
- `jq` (brew install jq)
- `git`

**Configuration:**
```json
{
  "statusLine": {
    "type": "script",
    "command": "./.claude/scripts/statusline-ccusage.sh"
  }
}
```

### 4. MCP Servers

**Pre-configured servers** (in `.claude/.mcp.json`):
- **Context7** : Real-time documentation (requires API key)
- **Atlassian** : Jira, Confluence, Bitbucket integration
- **Playwright** : Browser automation and testing

**Details:** See `docs/MCP.md` for installation and usage

---

## Configuration Management

### Permissions

**Allowed by default:**
```json
{
  "permissions": {
    "allow": [
      "WebSearch",
      "Bash(git commit:*)",
      "Bash(git add:*)",
      "Bash(npm:*)",
      "Bash(yarn:*)",
      "mcp__context7",
      "mcp__atlassian",
      "mcp__playwright"
    ],
    "deny": [
      "Read(.env*)"
    ]
  }
}
```

**Security Principles:**
- ✅ Protect all `.env*` files from reading
- ✅ Use wildcards for package managers (npm:*, yarn:*)
- ✅ Enable MCP servers explicitly
- ✅ Store API keys in settings.local.json (gitignored)

### Settings Hierarchy

1. **settings.json** (Team standards)
   - Versioned in Git
   - Shared with all team members
   - Minimal secure permissions
   - Common MCP servers

2. **settings.local.json** (Personal overrides)
   - Gitignored (.gitignore)
   - Personal MCP API keys
   - Additional permissions
   - Local scripts/tools

3. **Priority:** Local settings override team settings

---

## Development Workflow

### Adding New Configurations

1. **Test locally** in `.claude/`
2. **Update settings.json** if team-wide
3. **Use settings.local.json** if personal
4. **Update README.md and CLAUDE.md** if adding features
5. **Commit with descriptive message**

### Using /epct for Feature Development

**Recommended workflow:**
```bash
# 1. Launch EPCT
/epct implement user authentication with JWT

# 2. EXPLORE phase
# - explore-code agent discovers existing auth patterns
# - Finds middleware, routes, database schemas
# - Reports in isolated context

# 3. PLAN phase
# - Review proposed implementation
# - Ask clarifying questions if needed
# - Validate approach before coding

# 4. CODE phase
# - Implementation follows discovered patterns
# - Uses same naming conventions
# - Integrates with existing architecture

# 5. TEST phase
# - Run auth-related tests only
# - Verify new functionality works
# - If fails, return to PLAN
```

### Git Commit Workflow

**When committing changes:**
1. Use descriptive commit messages
2. Follow conventional commits format
3. Include Co-Authored-By for Claude
4. Reference issues/tickets if applicable

**Example:**
```bash
git commit -m "feat: add JWT authentication

Implements JWT-based authentication with refresh tokens.
- Add AuthService with token generation
- Add authentication middleware
- Add login/refresh endpoints
- Store refresh tokens in Redis (7-day TTL)

Resolves #123

🤖 Generated with Claude Code
Co-Authored-By: Claude <noreply@anthropic.com>"
```

---

## Best Practices for Claude

### When Working with This Configuration

1. **Always use /epct for non-trivial tasks**
   - Single file edits: Direct approach OK
   - Multi-file features: Use /epct
   - Refactoring: Use /epct
   - Bug fixes: Use /epct if complex

2. **Trust the explore-code agent**
   - Don't manually search during EXPLORE phase
   - Let the agent find patterns in isolation
   - Review agent findings before planning

3. **Respect the EPCT phases**
   - Don't skip EXPLORE for complex tasks
   - Always get plan approval before CODE
   - Only run relevant tests in TEST phase
   - Return to PLAN if tests fail

4. **Context preservation**
   - Use agents for exploration-heavy tasks
   - Keep main conversation focused
   - Summarize agent findings concisely

5. **Security awareness**
   - Never attempt to read .env files
   - Don't commit secrets to Git
   - Use settings.local.json for API keys
   - Validate permissions before tool use

### Code Generation Guidelines

1. **Match existing patterns**
   - Use discovered naming conventions
   - Follow project's code style
   - Reuse existing utilities

2. **Be explicit about changes**
   - List files to be modified
   - Explain architectural decisions
   - Note potential breaking changes

3. **Test appropriately**
   - Run tests related to changes
   - Don't run entire test suite
   - Explain test failures clearly

---

## Known Issues

### Marketplace Installation (⚠️ Experimental)

**Problem:** Plugin marketplace installation is incomplete
- ❌ Agents don't install to `~/.claude/agents/`
- ❌ Scripts don't install to `~/.claude/scripts/`
- ✅ Commands install correctly

**Workaround:** Use manual installation (Method 2)

**Status:** Being investigated with Claude Code team

### Statusline Compatibility

**Issue:** May not work on all terminals
**Affected:** Some Windows terminals, older Git Bash versions
**Solution:** Check troubleshooting in README.md

### MCP Context7

**Issue:** Requires API key setup
**Solution:**
```bash
export CONTEXT7_API_KEY="your-key"
# Add to settings.local.json permissions
```

---

## Quick Reference

### Common Commands

```bash
# Launch EPCT workflow
/epct [description]

# Check statusline
# (automatically displays if configured)

# Verify installation
ls ~/.claude/agents/explore-code.md
ls ~/.claude/commands/epct.md
ls ~/.claude/scripts/statusline-ccusage.sh
```

### File Locations

- **Global config:** `~/.claude/`
- **Project config:** `./.claude/`
- **Personal config:** `./.claude/settings.local.json` (gitignored)
- **Documentation:** `./docs/`

### Links

- **Main README:** [README.md](README.md)
- **Complete guide:** [docs/GUIDE.md](docs/GUIDE.md)
- **MCP documentation:** [docs/MCP.md](docs/MCP.md)
- **Marketplace:** [Lingelo/lingelo-marketplace](https://github.com/Lingelo/lingelo-marketplace) (experimental)
- **Issues:** [GitHub Issues](https://github.com/Lingelo/claude-code-sample/issues)

---

**Last updated:** 2025-01-19
**Configuration version:** 1.0.0
**Maintained by:** Angelo LIMA ([@Lingelo](https://github.com/Lingelo))
