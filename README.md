# Essential Claude Skills

A beginner-friendly collection of Claude Skills for non-coders learning to build custom AI workflows that work for their specific context.

---

## About

This repository contains the fundamental skills I wish I had when learning to [build my Digital Brain](https://nuhgid.substack.com/p/day-1-build-your-digital-brain-in) with Claude Code. 

There are many steps in the vibe coding process that most developers fail to include in their documentation. So for non-coders like myself, they may struggle with effectively using AI and waste precious time on the boring technical details.

Each skill in this repo is designed to help non-coders by outlining the processes I took to achieve an outcome and to share it in an accessible way.

---

## What Are Skills?

Skills are YAML + Markdown files that give Claude specialised knowledge and workflows for specific tasks. They extend Claude's capabilities by:

Skills can be executed either via certain trigger words specified within each  `SKILL.md` file, or execute automatically when invoked via `/skill-name` slash commands.

---

## Available Skills

| Skill                                                             | Description                                                                       | When to Use                                                                            |
| ----------------------------------------------------------------- | --------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| [google-workspace-credential](skills/google-workspace-credential) | Guide through creating OAuth credentials in Google Cloud Console                  | First-time Google API setup, need OAuth Client ID/Secret                               |
| [google-workspace-auth](skills/google-workspace-auth)             | Authenticate Google Workspace accounts using OAuth and store credentials securely | Set up Gmail, Calendar, Drive, or Forms API access; configure multiple Google accounts |
| [api-store](skills/api-store)                                     | Guide for storing API keys securely in .env files with git protection             | Need to store API keys, integrate external services, or secure credential management   |
| [article-to-blog](skills/article-to-blog)                         | Publish markdown articles with images to GitHub Pages using Quartz v4            | Finished writing an article and want to publish it to your blog with automated deployment |
| [article-to-html](skills/article-to-html)                         | Convert published blog articles to HTML for Substack/Medium/LinkedIn             | Cross-post your blog articles to other platforms with proper image URLs and formatting |

---

## Installation

### Method 1: Direct Installation (Recommended for Non-Coders)

This assumes that you already have a repository that you'd like to install these skills into.

The easiest way for non-coders like myself is by copying the [link to this repo](https://github.com/nuhgid/essential-claude-skills), or a [specific folder](https://github.com/nuhgid/essential-claude-skills/tree/main/skills) for that skill.

You can enter this prompt into Claude Code, Codex, or other platforms:

```
Please install the skills from this repository into my .claude/skills directory:
https://github.com/nuhgid/essential-claude-skills

Make sure to copy all SKILL.md files and any references folders to maintain the complete documentation.
```

Or for a specific skill:

```
Please install the [skill-name] skill from this folder into my .claude/skills directory:
https://github.com/nuhgid/essential-claude-skills/tree/main/skills/[skill-name]

Make sure to copy the SKILL.md file and any references folders.
```



### Method 2: Direct Clone into Claude Config

Clone directly into your Claude Code skills directory:

```bash
cd ~/.claude/skills
git clone https://github.com/yourusername/essential-claude-skills.git
```

Skills will be immediately available in Claude Code.

---

### Method 3: Individual Skill Copy

Copy specific skills you want to use:

```bash
# Copy to your .claude/skills directory
cp -r essential-claude-skills/skills/google-workspace-auth ~/.claude/skills/
cp -r essential-claude-skills/skills/google-workspace-credential ~/.claude/skills/
```

---

### Method 4: Manual Download

1. Navigate to the skill folder in this repository
2. Download the SKILL.md file and any reference folders
3. Create a corresponding folder in `~/.claude/skills/`
4. Paste files into the folder

---

## Usage

Once you've installed the skills in your repository, use these triggers to get Claude Code to help you with these tasks:

### Google Workspace Setup

**First-time OAuth setup:**
```
/google-workspace-credential
```
This walks you through Google Cloud Console to create OAuth credentials.

**Authenticate accounts:**
```
/google-workspace-auth
```
This authenticates your Google accounts and stores credentials securely in keyring.

### API Key Storage

**Store API keys securely:**
```
/api-store
```
This guides you through secure API key storage in `.env` files with git protection.

### Blog Publishing Workflow

**Publish articles to GitHub Pages:**
```
/article-to-blog
```
This walks you through:
- Setting up Quartz v4 blog (if needed)
- Organizing your article files
- Publishing to GitHub Pages
- Automated deployment via git

**Convert articles to HTML for cross-posting:**
```
/article-to-html
```
This converts your published blog articles to HTML for Substack, Medium, and LinkedIn.

**What you'll learn:**
- Quartz v4 static site generator setup
- GitHub Pages deployment
- Article organization patterns
- Markdown to HTML conversion
- Obsidian syntax support (callouts, highlights, wikilinks)
- Cross-platform publishing workflow

---

## File Structure

```
essential-claude-skills/
├── README.md
└── skills/
    ├── google-workspace-credential/
    │   └── SKILL.md
    ├── google-workspace-auth/
    │   ├── SKILL.md
    │   └── references/
    │       ├── implementation.md
    │       └── troubleshooting.md
    ├── api-store/
    │   ├── SKILL.md
    │   └── references/
    │       └── common-services.md
    ├── article-to-blog/
    │   ├── SKILL.md
    │   └── references/
    │       ├── quartz-v4-setup.md
    │       └── file-organization.md
    └── article-to-html/
        ├── SKILL.md
        └── references/
            └── supported-markdown.md
```

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
3. References folder for additional documentation (when needed)
4. Clear prerequisites and workflow steps
5. Real examples, not synthetic scenarios

---

## License

MIT License - Feel free to use these skills in your own projects.
