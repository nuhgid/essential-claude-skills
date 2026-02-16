# Essential Claude Skills

A beginner-friendly collection of Claude Code skills for developers learning to build custom AI workflows.

---

## About

This repository contains the fundamental skills I wish I had when learning to write code with Claude. Each skill is documented to help you understand not just *what* it does, but *how* it works—making them ideal learning resources for building your own custom workflows.

These skills focus on practical automation patterns you'll encounter when working with APIs, handling authentication, and building real-world integrations.

**Related Resources:**
- [Digital Brain System](https://github.com/nuhgid/bydb-nuhgid-demo) - Complete workflow automation examples
- [Claude Code Documentation](https://docs.anthropic.com/claude-code) - Official Claude Code reference

---

## What Are Skills?

Skills are YAML + Markdown files that give Claude specialized knowledge and workflows for specific tasks. They extend Claude's capabilities by:

- Defining clear workflows with step-by-step instructions
- Handling authentication and API integrations
- Automating repetitive setup processes
- Providing consistent, repeatable patterns

Skills execute automatically when invoked via `/skill-name` commands, making complex multi-step workflows accessible through simple commands.

---

## Available Skills

| Skill | Description | When to Use |
|-------|-------------|-------------|
| [google-workspace-credential](skills/google-workspace-credential) | Guide through creating OAuth credentials in Google Cloud Console | First-time Google API setup, need OAuth Client ID/Secret |
| [google-workspace-auth](skills/google-workspace-auth) | Authenticate Google Workspace accounts using OAuth and store credentials securely | Set up Gmail, Calendar, Drive, or Forms API access; configure multiple Google accounts |

---

## Installation

### Method 1: Direct Clone into Claude Config

Clone directly into your Claude Code skills directory:

```bash
cd ~/.claude/skills
git clone https://github.com/yourusername/essential-claude-skills.git
```

Skills will be immediately available in Claude Code.

---

### Method 2: Individual Skill Copy

Copy specific skills you want to use:

```bash
# Copy to your .claude/skills directory
cp -r essential-claude-skills/skills/google-workspace-auth ~/.claude/skills/
cp -r essential-claude-skills/skills/google-workspace-credential ~/.claude/skills/
```

---

### Method 3: Manual Download

1. Navigate to the skill folder in this repository
2. Download the SKILL.md file and any reference folders
3. Create corresponding folder in `~/.claude/skills/`
4. Paste files into the folder

---

## Skill Categories

### 🔐 Authentication & Credentials

**google-workspace-credential** - Create OAuth credentials in Google Cloud Console
**google-workspace-auth** - Authenticate Google accounts and store credentials securely in keyring

These skills work together in a two-step workflow:
1. Run `/google-workspace-credential` to generate OAuth Client ID and Secret
2. Run `/google-workspace-auth` to authenticate accounts and enable API access

**Why this matters:** Most beginners struggle with OAuth setup. These skills break down the process into clear, executable steps with security best practices built in.

---

## Usage Examples

### Google Workspace Setup Workflow

**First-time setup:**
```
/google-workspace-credential
```
This walks you through Google Cloud Console to create OAuth credentials.

**Authenticate accounts:**
```
/google-workspace-auth
```
This authenticates your Google accounts and stores credentials securely.

**What you'll learn:**
- OAuth 2.0 flow for desktop applications
- Secure credential storage with keyring
- Multi-account configuration patterns
- Environment variable management
- Google Cloud API enablement

---

## How Skills Work

Each skill follows this structure:

```markdown
---
name: skill-name
description: One-line trigger description
user-invokable: true
disable-model-invocation: false
---

# Skill Name

Clear explanation of what this skill does.

## Prerequisites
What you need before running this skill

## Workflow
Step-by-step execution plan

## Implementation Details
Technical specifics and patterns
```

**Key components:**
- **Frontmatter metadata** - Defines skill properties and invocation rules
- **Prerequisites** - Dependencies and requirements
- **Workflow** - Clear execution steps Claude follows
- **References** - Additional documentation for complex workflows

---

## Learning from These Skills

### For Beginners

**Start here:**
1. Read the SKILL.md files to understand the workflow structure
2. Notice how complex processes are broken into discrete steps
3. Observe security patterns (template files, keyring storage, no credentials in chat)
4. See how skills chain together (credential → auth → usage)

**Key patterns to study:**
- Step-by-step user guidance with clear instructions
- Separation of credential creation and authentication
- Template-based secure credential handling
- Multi-account configuration management
- Error handling and troubleshooting guidance

### For Intermediate Developers

**Focus on:**
- Workflow orchestration patterns
- User interaction design (when to ask, when to execute)
- Security-first credential management
- Configuration file generation (YAML, .env templates)
- Integration with external tools (make commands, OAuth flows)

---

## Contributing

Contributions welcome. Submit pull requests with:
- Clear skill documentation following existing patterns
- Real-world use cases demonstrated
- Security best practices maintained
- Beginner-friendly explanations

**Skill submission guidelines:**
1. One skill per folder
2. SKILL.md with frontmatter metadata
3. References folder for additional documentation
4. Clear prerequisites and workflow steps
5. Real examples, not synthetic scenarios

---

## Philosophy

> "Build the tools you wish existed when you started."

These skills prioritize:
- **Clarity over cleverness** - Straightforward workflows beat clever shortcuts
- **Security by default** - Credentials never exposed, templates over direct input
- **Real-world patterns** - Actual workflows, not hypothetical examples
- **Learning-focused** - Designed to teach through structure and documentation

---

## Maintenance

**Skill health:**
- Each skill tested with real Google Workspace accounts
- Documentation updated based on actual usage
- Security patterns reviewed for best practices
- Breaking changes documented in release notes

**Version compatibility:**
- Claude Code version: Latest stable
- Google Cloud APIs: Current production versions
- OAuth 2.0: Desktop application flow

---

## Notes

These skills save setup time and demonstrate reusable patterns. They're not "set and forget"—read through them, understand the workflows, then adapt them to your own use cases.

**Remember:** Skills handle the repeatable. You handle the strategic. Understanding how these work makes you better at building your own automation.
