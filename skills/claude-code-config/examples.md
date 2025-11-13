# Claude Code Configuration Examples

## Real-World Skills

### 1. PDF Processing Skill

```yaml
---
name: pdf-processor
description: Extract text and tables from PDF files, fill forms, merge PDFs. Use when working with PDFs or document extraction.
allowed-tools: [Read, Write, Bash]
---

# PDF Processor

When processing PDFs:

1. Use `pdftotext` for text extraction
2. Use `pdftk` for merging and splitting
3. Use `qpdf` for form filling

## Commands

- Extract text: `pdftotext input.pdf output.txt`
- Merge PDFs: `pdftk file1.pdf file2.pdf cat output merged.pdf`
- Extract pages: `pdftk input.pdf cat 1-3 output pages.pdf`
```

### 2. API Documentation Skill

```yaml
---
name: api-docs-generator
description: Generate API documentation from code, create OpenAPI specs, document REST endpoints. Use when documenting APIs or creating API specs.
---

# API Documentation Generator

When generating API documentation:

1. Scan code for route definitions
2. Extract parameter types and descriptions
3. Generate OpenAPI/Swagger specs
4. Create markdown documentation

## Output Format

Use OpenAPI 3.0 specification with:
- Clear endpoint descriptions
- Request/response examples
- Authentication requirements
- Error responses
```

### 3. Database Migration Skill

```yaml
---
name: db-migrations
description: Create and manage database migrations for PostgreSQL, MySQL, SQLite. Use when creating database migrations or schema changes.
allowed-tools: [Read, Write, Bash]
---

# Database Migration Manager

When creating migrations:

1. Follow naming: `YYYYMMDDHHMMSS_description.sql`
2. Include both up and down migrations
3. Make migrations idempotent
4. Add rollback instructions

## Migration Template

```sql
-- Up Migration
CREATE TABLE IF NOT EXISTS users (
  id SERIAL PRIMARY KEY,
  email VARCHAR(255) UNIQUE NOT NULL,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Down Migration
DROP TABLE IF EXISTS users;
```
```

## Real-World Subagents

### 1. Security Auditor

```markdown
---
name: security-auditor
description: Audit code for security vulnerabilities, check for XSS, SQL injection, CSRF, authentication issues
tools: Read, Grep, Glob
model: sonnet
---

You are a security expert focused on finding vulnerabilities.

## Check List

1. **Input Validation**
   - Check all user inputs are validated
   - Look for SQL injection risks
   - Find XSS vulnerabilities

2. **Authentication**
   - Verify password hashing (bcrypt, argon2)
   - Check session management
   - Look for authentication bypasses

3. **Authorization**
   - Verify access controls
   - Check for IDOR vulnerabilities
   - Review permission checks

4. **Data Protection**
   - Find hardcoded secrets
   - Check for sensitive data exposure
   - Verify encryption usage

5. **Dependencies**
   - Check for known vulnerabilities
   - Review package versions
   - Flag outdated dependencies

Prioritize critical and high severity issues.
```

### 2. Performance Optimizer

```markdown
---
name: performance-optimizer
description: Analyze and optimize code performance, database queries, React renders, bundle size
tools: Read, Grep, Glob, Bash
model: sonnet
---

You are a performance optimization expert.

## Analysis Steps

1. **Identify Bottlenecks**
   - Profile code execution
   - Find slow database queries
   - Detect N+1 query problems
   - Check for unnecessary re-renders

2. **Database Optimization**
   - Add missing indexes
   - Optimize query structure
   - Implement caching strategies
   - Use prepared statements

3. **Frontend Optimization**
   - Lazy load components
   - Implement code splitting
   - Optimize images
   - Reduce bundle size
   - Use React.memo and useMemo

4. **Caching**
   - Add appropriate cache headers
   - Implement Redis caching
   - Use CDN for static assets

Provide before/after benchmarks when possible.
```

### 3. Test Generator

```markdown
---
name: test-generator
description: Generate unit tests, integration tests, E2E tests for TypeScript, JavaScript, Python code
tools: Read, Write, Grep
model: sonnet
---

You generate comprehensive test suites.

## Test Coverage

For each function/component:
1. Happy path tests
2. Edge cases
3. Error conditions
4. Boundary values

## Frameworks

- **React**: Jest + React Testing Library
- **Node.js**: Jest + Supertest
- **Python**: pytest
- **E2E**: Playwright

## Test Structure

```typescript
describe('ComponentName', () => {
  it('should handle valid input', () => {
    // Arrange
    // Act
    // Assert
  });

  it('should handle edge cases', () => {});
  it('should handle errors', () => {});
});
```

Aim for 80%+ code coverage.
```

## Advanced Hook Configurations

### 1. Auto-format on Save

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(**/*.{ts,tsx,js,jsx})",
        "hooks": [
          {
            "type": "command",
            "command": "npx prettier --write {file_path}"
          }
        ]
      },
      {
        "matcher": "Write(**/*.py)",
        "hooks": [
          {
            "type": "command",
            "command": "black {file_path}"
          }
        ]
      },
      {
        "matcher": "Write(**/*.go)",
        "hooks": [
          {
            "type": "command",
            "command": "gofmt -w {file_path}"
          }
        ]
      }
    ]
  }
}
```

### 2. Pre-commit Validation

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write(**/*.env)",
        "hooks": [
          {
            "type": "block",
            "message": "Cannot modify .env files. Use .env.example instead."
          }
        ]
      },
      {
        "matcher": "Bash(git push*)",
        "hooks": [
          {
            "type": "command",
            "command": "npm test"
          }
        ]
      }
    ]
  }
}
```

### 3. Notifications

```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "osascript -e 'display notification \"Claude Code task completed\" with title \"Claude Code\"'"
          }
        ]
      }
    ],
    "Notification": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "osascript -e 'display notification \"Claude Code needs input\" with title \"Claude Code\"'"
          }
        ]
      }
    ]
  }
}
```

### 4. Git Automation

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(**/*.md)",
        "hooks": [
          {
            "type": "command",
            "command": "git add {file_path} && git commit -m 'docs: update documentation'"
          }
        ]
      }
    ]
  }
}
```

## Complex Permission Configurations

### 1. Development Environment

```json
{
  "permissions": {
    "allow": [
      "Read(**/*.{ts,tsx,js,jsx,py,go,rs})",
      "Write(**/*.{ts,tsx,js,jsx,py,go,rs})",
      "Bash(npm *)",
      "Bash(yarn *)",
      "Bash(pnpm *)",
      "Bash(bun *)",
      "Bash(git status)",
      "Bash(git diff*)",
      "Bash(git log*)",
      "Bash(git show*)",
      "WebFetch(domain:*)",
      "mcp__*"
    ],
    "ask": [
      "Bash(git push*)",
      "Bash(git commit*)",
      "Write(.github/**)",
      "Write(package.json)",
      "Write(tsconfig.json)"
    ],
    "deny": [
      "Read(.env)",
      "Read(**/*.key)",
      "Read(**/*.pem)",
      "Write(.env)",
      "Bash(rm -rf*)",
      "Bash(sudo *)",
      "Bash(npm publish*)"
    ]
  }
}
```

### 2. Production Environment

```json
{
  "permissions": {
    "allow": [
      "Read(**/*.{ts,tsx,js,jsx})"
    ],
    "ask": [
      "Write(**/*)",
      "Bash(*)"
    ],
    "deny": [
      "Read(.env*)",
      "Read(secrets/**)",
      "Write(.env*)",
      "Write(config/production.json)",
      "Bash(git push*)",
      "Bash(npm *)",
      "Bash(rm *)",
      "Bash(sudo *)"
    ]
  }
}
```

### 3. Read-Only Documentation Mode

```json
{
  "permissions": {
    "allow": [
      "Read(**/*.md)",
      "Read(**/*.{ts,tsx,js,jsx})",
      "Grep",
      "Glob",
      "WebFetch(domain:docs.*)",
      "mcp__context7__*"
    ],
    "deny": [
      "Write(**/*)",
      "Edit(**/*)",
      "Bash(*)"
    ]
  }
}
```

## MCP Server Configurations

### 1. Full Stack Setup

```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp", "--api-key", "your-api-key"]
    },
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://localhost:5432/mydb"
      }
    },
    "playwright": {
      "command": "npx",
      "args": ["-y", "@executeautomation/playwright-mcp-server"]
    },
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/allowed/directory"]
    }
  }
}
```

### 2. API Development Setup

```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp"]
    },
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "${DATABASE_URL}"
      }
    },
    "redis": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-redis"],
      "env": {
        "REDIS_URL": "redis://localhost:6379"
      }
    }
  }
}
```

## Project-Specific Configurations

### Next.js Project (.claude/settings.json)

```json
{
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
    ]
  },
  "permissions": {
    "allow": [
      "Read(**/*.{ts,tsx,css,json})",
      "Bash(npm run*)",
      "Bash(bun *)",
      "mcp__context7__*"
    ],
    "deny": [
      "Read(.env.local)",
      "Write(next.config.js)"
    ]
  }
}
```

### Python Django Project

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(**/*.py)",
        "hooks": [
          {
            "type": "command",
            "command": "black {file_path} && isort {file_path}"
          }
        ]
      }
    ]
  },
  "permissions": {
    "allow": [
      "Read(**/*.py)",
      "Bash(python manage.py*)",
      "Bash(pytest*)",
      "mcp__postgres__*"
    ]
  },
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://localhost:5432/django_db"
      }
    }
  }
}
```
