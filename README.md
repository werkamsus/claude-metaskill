<div align="center">

# Claude MetaSkill

> **A Claude Code skill that makes Claude an expert at configuring Claude Code**

<img src="assets/hero.png" alt="Claude MetaSkill" width="600">

A meta-skill that Claude Code uses to expertly guide you through creating skills, subagents, hooks, permissions, and MCP server integrations. When installed, Claude becomes your configuration expert.

[![Claude Code](https://img.shields.io/badge/Claude-Code-8A63D2?logo=anthropic)](https://claude.ai/code)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

</div>

## What This Skill Does

When installed, Claude Code automatically uses this skill to provide expert guidance on:

- ✨ **Creating Skills** - File structure, SKILL.md format, best practices
- 🤖 **Building Subagents** - YAML configuration, tool permissions, model selection
- 🪝 **Configuring Hooks** - Auto-format on save, notifications, validation
- 🔐 **Managing Permissions** - Allow/ask/deny patterns, security best practices
- 🔌 **Setting Up MCP Servers** - Context7, Playwright, Postgres integration
- 📚 **Real-World Examples** - Production-ready configurations and patterns
- 🔧 **Troubleshooting** - Common issues and solutions

## Installation

### Option 1: Via SkillsMP.com (Recommended)

1. Visit [SkillsMP.com](https://skillsmp.com)
2. Search for "claude-metaskill"
3. Click install and follow instructions

### Option 2: Via Plugin Marketplace

```bash
# Add the marketplace
/plugin marketplace add werkamsus/claude-metaskill

# Install the skill
/plugin install claude-code-config
```

### Option 3: Manual Installation

**Global (all projects):**
```bash
git clone https://github.com/werkamsus/claude-metaskill.git
cp -r claude-metaskill/skills/claude-code-config ~/.claude/skills/
```

**Project-specific:**
```bash
git clone https://github.com/werkamsus/claude-metaskill.git
cp -r claude-metaskill/skills/claude-code-config .claude/skills/
```

## Usage

Once installed, the skill activates automatically when you ask about Claude Code configuration:

```
"How do I create a new skill?"
"Set up a subagent for code review"
"Configure Context7 MCP"
"Add a hook to auto-format TypeScript files"
"Set up permissions to block .env files"
```

Or explicitly invoke it:
```
"Use the claude-code-config skill to help me create a test generator subagent"
```

## What's Included

### Main Guide (SKILL.md)
- Complete configuration reference
- Directory structure and file formats
- Step-by-step instructions
- Quick reference tables
- Troubleshooting section

### Examples Library (examples.md)
- Real-world skills (PDF processor, API docs generator, database migrations)
- Production subagents (security auditor, performance optimizer, test generator)
- Advanced hook configurations (auto-format, validation, notifications)
- Complex permission patterns (dev, production, read-only modes)
- MCP server setups (full stack, API development)
- Project-specific configs (Next.js, Django)

## Example Scenarios

### Creating Your First Skill

```
You: "I want to create a skill for processing markdown files"
Claude: [Uses claude-code-config skill]

        Let me help you create a markdown processing skill.

        First, I'll create the directory structure:
        ~/.claude/skills/markdown-processor/SKILL.md

        [Creates skill with proper YAML frontmatter and instructions]
```

### Setting Up Context7 MCP

```
You: "Help me set up Context7 to get up-to-date library docs"
Claude: [Uses claude-code-config skill]

        I'll help you install and configure Context7 MCP server.

        Here are the steps:
        1. Add to your settings.json...
        2. Get an API key from context7.com...
        3. Configure permissions...
```

### Building a Code Review Subagent

```
You: "Create a subagent that reviews code for security issues"
Claude: [Uses claude-code-config skill]

        I'll create a security-focused code review subagent.

        [Creates ~/.claude/agents/security-reviewer.md with:
         - Proper YAML configuration
         - Security checklist
         - Tool restrictions
         - Clear instructions]
```

## Documentation Structure

```
claude-metaskill/
├── skills/claude-code-config/
│   ├── SKILL.md           # Main reference guide
│   └── examples.md        # Real-world examples
├── .claude-plugin/
│   └── marketplace.json   # Plugin marketplace config
├── README.md              # This file
└── LICENSE               # MIT License
```

## Contributing

Found an issue or want to add examples? Contributions welcome!

1. Fork the repository
2. Create your feature branch
3. Add your examples or improvements
4. Submit a pull request

## Topics Covered

- **Skills**: Creation, structure, best practices, activation triggers
- **Subagents**: YAML frontmatter, tool permissions, model selection
- **Hooks**: PreToolUse, PostToolUse, Stop, Notification events
- **Permissions**: Allow/ask/deny patterns, security configurations
- **MCP Servers**: Context7, Playwright, Postgres, custom servers
- **Settings**: Hierarchy, precedence, environment variables
- **Commands**: /config, /agents, /plugin management

## Official Documentation Links

- [Claude Code Skills](https://code.claude.com/docs/en/skills)
- [Subagents Guide](https://code.claude.com/docs/en/sub-agents)
- [Settings Reference](https://code.claude.com/docs/en/settings)
- [Plugin Marketplaces](https://code.claude.com/docs/en/plugin-marketplaces)

## License

MIT License - feel free to use, modify, and share!

## Author

Created by Nick Raziborsky

## Acknowledgments

Built with knowledge from:
- Anthropic's official Claude Code documentation
- Community best practices
- Real-world configuration patterns

---

**Make Claude Code truly yours** - master configuration with Claude MetaSkill ✨
