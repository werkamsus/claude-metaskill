---
name: claude-code-config
description: Expert guidance for configuring Claude Code including creating skills, subagents, hooks, permissions, and MCP servers like Context7. Use when asked about Claude Code configuration, settings, creating custom skills, setting up subagents, configuring hooks, managing permissions, or integrating MCP servers.
---

# Claude Code Configuration Expert

This skill provides comprehensive guidance for configuring and extending Claude Code through skills, subagents, hooks, permissions, and MCP servers.

## Official Documentation

- **Skills**: https://code.claude.com/docs/en/skills
- **Subagents**: https://code.claude.com/docs/en/sub-agents
- **Settings**: https://code.claude.com/docs/en/settings
- **Hooks**: https://code.claude.com/docs/en/hooks
- **MCP Servers**: https://code.claude.com/docs/en/mcp

## Directory Structure

```
~/.claude/                    # Global (user-level) configuration
├── settings.json            # Global settings, hooks, permissions
├── skills/                  # Global skills (all projects)
│   └── skill-name/
│       └── SKILL.md
├── agents/                  # Global subagents (all projects)
│   └── agent-name.md
└── plugins/                 # Installed plugins

.claude/                     # Project-level configuration (overrides global)
├── settings.json           # Project settings
├── settings.local.json     # Local project settings (gitignored)
├── skills/                 # Project-specific skills
└── agents/                 # Project-specific subagents
```

---

## Creating Skills

### Skill File Structure

```
~/.claude/skills/skill-name/
├── SKILL.md                # Required: Main skill file
├── examples.md             # Optional: Code examples
├── reference.md            # Optional: Additional documentation
└── templates/              # Optional: Template files
```

### SKILL.md Format

```yaml
---
name: skill-identifier
description: What the skill does and when Claude should use it. Be specific and include trigger terms users would mention.
allowed-tools: [Read, Write, Edit]  # Optional: Restrict tools
---

# Skill Title

## Instructions
Detailed instructions for Claude to follow when this skill is activated.

## Examples
Show concrete examples of how to use this skill.
```

### Best Practices for Skills

1. **Make descriptions discovery-friendly**: Include concrete trigger terms
   - Bad: "helps with documents"
   - Good: "Extract text from PDFs, fill forms, merge documents. Use when working with PDFs or document extraction."

2. **Keep skills focused**: One capability per skill

3. **Test thoroughly**: Ask questions matching your description to verify activation

4. **Use relative paths**: Reference additional files from SKILL.md

5. **Limit tools when needed**: Use `allowed-tools` for read-only or security-sensitive workflows

---

## Creating Subagents

### Subagent File Structure

Subagents are Markdown files with YAML frontmatter:

```markdown
---
name: my-subagent
description: When this subagent should be invoked
tools: Read, Write, Grep, Bash
model: sonnet
---

System prompt for the subagent goes here.
Provide detailed instructions on how the subagent should behave.
```

### Configuration Fields

- **name** (required): Lowercase letters and hyphens only
- **description** (required): Natural language description of purpose
- **tools** (optional): Comma-separated list of allowed tools
  - Omit to inherit all tools from main thread (including MCP tools)
  - Use `/agents` command to see all available tools
- **model** (optional): `sonnet`, `opus`, `haiku`, or `inherit`

### Storage Locations

- **Global**: `~/.claude/agents/agent-name.md` (all projects)
- **Project**: `.claude/agents/agent-name.md` (current project, highest priority)

Project-level subagents override global ones with the same name.

### Example Subagent

```markdown
---
name: code-reviewer
description: Reviews code for bugs, security issues, and best practices
tools: Read, Grep, Glob
model: sonnet
---

You are a code review expert. When reviewing code:

1. Check for security vulnerabilities (XSS, SQL injection, etc.)
2. Verify error handling
3. Look for performance issues
4. Suggest improvements
5. Provide actionable feedback

Focus on critical issues first, then optimizations.
```

---

## Configuring Hooks

Hooks run custom commands before or after tool executions. Configure in `~/.claude/settings.json`:

### Hook Types

- **PreToolUse**: Runs before a tool is executed
- **PostToolUse**: Runs after a tool completes
- **Stop**: Runs when Claude stops
- **Notification**: Runs when Claude sends a notification

### Hook Configuration Format

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(**/*.py)",
        "hooks": [
          {
            "type": "command",
            "command": "black {file_path}"
          }
        ]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Write(.env)",
        "hooks": [
          {
            "type": "block",
            "message": "Cannot modify .env files"
          }
        ]
      }
    ]
  }
}
```

### Common Hook Examples

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(**/*.ts)",
        "hooks": [
          {
            "type": "command",
            "command": "npx prettier --write {file_path}"
          }
        ]
      }
    ],
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "terminal-notifier -message 'Task completed' -title 'Claude Code'"
          }
        ]
      }
    ]
  }
}
```

### Variables Available in Hooks

- `{file_path}`: The file being operated on
- `{tool_name}`: Name of the tool being used

---

## Configuring Permissions

Permissions control which tools Claude can use. Configure in `settings.json`:

### Permission Types

- **allow**: Auto-approve these tool operations
- **ask**: Require confirmation before execution
- **deny**: Block these operations completely

### Permission Format

```json
{
  "permissions": {
    "allow": [
      "Read(**/*.md)",
      "Bash(git status)",
      "WebFetch(domain:docs.anthropic.com)"
    ],
    "deny": [
      "Read(.env)",
      "Read(secrets/**)",
      "Write(.env)",
      "Bash(rm -rf*)"
    ]
  }
}
```

### Common Permission Patterns

```json
{
  "permissions": {
    "allow": [
      "Read(**/*.{ts,js,tsx,jsx})",
      "Bash(npm *)",
      "Bash(git *)",
      "mcp__context7__*"
    ],
    "deny": [
      "Read(.env)",
      "Read(**/*.key)",
      "Read(config/secrets.json)",
      "Bash(rm *)",
      "Bash(sudo *)"
    ]
  }
}
```

---

## Setting Up MCP Servers

### Context7 MCP (Up-to-date Library Documentation)

Context7 provides current, version-specific documentation for 20,000+ libraries.

#### Installation

**Method 1: CLI (Recommended)**
```bash
claude mcp add context7 -- npx -y @upstash/context7-mcp --api-key YOUR_API_KEY
```

**Method 2: Manual Configuration**

Add to `~/.claude/settings.json`:

```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp", "--api-key", "YOUR_API_KEY"]
    }
  }
}
```

#### Getting API Key

1. Visit https://context7.com/dashboard
2. Create account
3. Generate API key (optional but recommended for higher rate limits)

#### Usage

Use Context7 tools in conversations:
- `mcp__context7__resolve-library-id`: Find library ID
- `mcp__context7__get-library-docs`: Fetch documentation

Or ask Claude to use Context7:
```
"Use Context7 to get the latest React hooks documentation"
```

#### Permissions for Context7

```json
{
  "permissions": {
    "allow": [
      "mcp__context7__resolve-library-id",
      "mcp__context7__get-library-docs"
    ]
  }
}
```

### Other Common MCP Servers

**Playwright (Browser Automation)**
```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["-y", "@executeautomation/playwright-mcp-server"]
    }
  }
}
```

**Postgres (Database Access)**
```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://user:password@localhost:5432/dbname"
      }
    }
  }
}
```

---

## Complete Settings Example

```json
{
  "model": "sonnet[1m]",
  "alwaysThinkingEnabled": true,

  "permissions": {
    "allow": [
      "Read(**/*.{ts,tsx,js,jsx,md})",
      "Bash(git *)",
      "Bash(npm *)",
      "mcp__context7__*",
      "WebFetch(domain:*)"
    ],
    "deny": [
      "Read(.env)",
      "Read(**/*.key)",
      "Bash(rm -rf*)",
      "Bash(sudo *)"
    ]
  },

  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(**/*.{ts,tsx})",
        "hooks": [
          {
            "type": "command",
            "command": "npx prettier --write {file_path}"
          }
        ]
      }
    ],
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "terminal-notifier -message 'Claude Code completed' -title 'Claude Code'"
          }
        ]
      }
    ]
  },

  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp", "--api-key", "your-api-key"]
    }
  },

  "enabledPlugins": {
    "frontend-design@claude-code-plugins": true
  }
}
```

---

## Settings Precedence Hierarchy

Settings are loaded in this order (highest priority first):

1. **Enterprise managed policies** (highest)
2. **Command-line arguments**
3. **Local project settings** (`.claude/settings.local.json`)
4. **Shared project settings** (`.claude/settings.json`)
5. **User settings** (`~/.claude/settings.json`, lowest)

---

## Useful Commands

- `/config` - View current configuration
- `/agents` - Manage subagents and view available tools
- `claude mcp add <name>` - Install MCP server
- `claude mcp list` - List installed MCP servers

---

## Quick Reference: When to Use What

| Need | Use |
|------|-----|
| Reusable instructions across projects | Global Skill |
| Project-specific behavior | Project Skill |
| Specialized task handler | Subagent |
| Auto-format on save | PostToolUse Hook |
| Prevent dangerous operations | Deny Permission |
| Auto-approve safe operations | Allow Permission |
| Up-to-date library docs | Context7 MCP |
| Browser automation | Playwright MCP |
| Database queries | Postgres MCP |

---

## Troubleshooting

### Skill Not Activating
- Check description includes trigger terms
- Test with questions matching description
- Verify skill name is lowercase with hyphens only

### Subagent Not Working
- Ensure YAML frontmatter is valid
- Check name doesn't conflict with global subagent
- Verify tools list includes needed tools

### Hook Not Running
- Check matcher pattern matches tool use
- Verify command is executable
- Test command manually first

### MCP Server Not Available
- Check server is in `mcpServers` config
- Verify command and args are correct
- Look for errors in Claude Code logs

### Permission Issues
- Check deny rules don't block needed operations
- Verify allow rules are specific enough
- Remember deny takes precedence over allow
