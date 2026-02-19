---
name: article-to-html
description: Convert published blog articles to HTML for copy-paste to Substack, Medium, and LinkedIn. Supports Obsidian markdown with callouts, highlights, and wikilinks.
user-invocable: true
disable-model-invocation: false
---

# Article to HTML Converter

Convert your published GitHub Pages blog articles to clean HTML for Substack, Medium, and LinkedIn.

## Prerequisites

1. Article must be published to your blog first (use `/article-to-blog`)
2. Blog must be deployed to GitHub Pages
3. Images must be publicly accessible

## What This Skill Does

1. Reads article from your blog repository
2. Converts Markdown → HTML (including Obsidian syntax)
3. Replaces image references with public GitHub Pages URLs
4. Generates styled HTML
5. Opens in browser for copy-paste

## Workflow

### Step 1: Locate Blog Repository

Ask: "Where is your blog repository?"

**Example:** `~/Documents/blog` or `/Users/username/projects/blog`

**Verify Quartz structure:**
```bash
ls [blog-path]/content/articles
```

If not found:
```
❌ Blog articles directory not found

Expected: [blog-path]/content/articles/

Have you published any articles yet?
Use /article-to-blog to publish your first article.
```

### Step 2: List Available Articles

Show published articles:
```bash
ls [blog-path]/content/articles/
```

```
📁 Available articles:
  - how-to-build-with-claude
  - my-first-post
  - learning-to-code

Which article would you like to convert to HTML?
```

### Step 3: Select Article

User provides article slug or search term.

**Search by:**
- Exact slug: `how-to-build-with-claude`
- Partial match: `claude` (finds `how-to-build-with-claude`)
- Title keywords: `build claude`

**Find matching article folder:**
```bash
find [blog-path]/content/articles/ -name "*[search-term]*"
```

**Verify `index.md` exists:**
```
✓ Found: how-to-build-with-claude/index.md
```

### Step 4: Get GitHub Pages URL

Ask: "What's your GitHub Pages URL?"

**Example:** `https://username.github.io/blog`

**Generate article URL:**
```
Article: https://username.github.io/blog/articles/how-to-build-with-claude/
Images: https://username.github.io/blog/articles/how-to-build-with-claude/[image.png]
```

### Step 5: Read and Process Markdown

**Read `index.md`:**
```bash
cat [blog-path]/content/articles/[slug]/index.md
```

**Strip YAML frontmatter:**
```yaml
---
title: Article Title
date: 2026-02-19
tags: [tech, tutorial]
---
```

Remove everything between `---` markers.

**Extract title:**
- Try YAML `title:` field first
- Fallback to first `# Heading`
- Fallback to folder name

### Step 6: Convert Images to Public URLs

**Find all image references:**
- Standard markdown: `![alt](image.png)`
- Markdown with path: `![alt](attachments/image.png)`
- Obsidian wikilinks: `![[image.png]]`
- Obsidian with size: `![[image.png|300]]`

**Replace with HTML img tags:**
```html
<img src="https://username.github.io/blog/articles/[slug]/image.png" alt="image" />
```

**Handle spaces in filenames:**
- Original: `my image.png`
- URL: `my-image.png` (Quartz sanitizes to hyphens)

### Step 7: Convert Markdown to HTML

**Process in order:**
1. Remove Obsidian comments: `%%comment%%`
2. Convert callouts: `> [!note]` → styled divs
3. Convert horizontal rules: `---` → `<hr>`
4. Convert blockquotes: `> text` → `<blockquote>`
5. Convert task lists: `- [ ]` and `- [x]`
6. Convert unordered lists: `- item`
7. Convert ordered lists: `1. item`
8. Convert headers: `# H1` → `<h1>`
9. Convert code blocks: ` ```code``` `
10. Convert inline code: `` `code` ``
11. Convert links: `[text](url)` → `<a>`
12. Convert highlights: `==text==` → `<mark>`
13. Convert bold + italic: `***text***`
14. Convert bold: `**text**` → `<strong>`
15. Convert italic: `*text*` → `<em>`
16. Convert strikethrough: `~~text~~` → `<del>`
17. Convert wikilinks: `[[Note|Display]]` → plain text
18. Wrap paragraphs: `<p>`

See [references/supported-markdown.md](references/supported-markdown.md) for complete syntax support.

### Step 8: Generate HTML Template

**Wrap content in styled template:**

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>[Article Title]</title>
    <style>
        body {
            font-family: Georgia, serif;
            line-height: 1.6;
            max-width: 700px;
            margin: 40px auto;
            padding: 0 20px;
        }
        /* ... complete styles for all elements ... */
    </style>
</head>
<body>
[Converted HTML content]
</body>
</html>
```

**Styles included for:**
- Typography (headers, paragraphs, lists)
- Code blocks and inline code
- Images (responsive)
- Blockquotes
- Callouts (colored boxes)
- Task lists (checkboxes)
- Highlights, strikethrough
- Links

### Step 9: Save and Open

**Save to temporary location:**
```bash
/tmp/[slug]_substack.html
```

**Open in default browser:**
```bash
open /tmp/[slug]_substack.html
```

**Show instructions:**
```
✅ HTML generated: /tmp/[slug]_substack.html
📋 Opened in browser

Next steps:
1. Copy entire page (Cmd+A, Cmd+C on Mac / Ctrl+A, Ctrl+C on Windows)
2. Open Substack/Medium/LinkedIn editor
3. Paste (Cmd+V / Ctrl+V)
4. Images will load from: https://username.github.io/blog/articles/[slug]/
5. Publish!

✨ Obsidian features preserved:
   - Callouts (note, warning, tip, etc.)
   - Wikilinks [[Note|Display]]
   - Highlights ==text==
   - Task lists - [ ] and - [x]
   - Comments %%hidden%%
```

## Supported Markdown Syntax

### Standard Markdown

- **Headers:** `# H1`, `## H2`, `### H3`, `#### H4`
- **Bold:** `**text**` or `__text__`
- **Italic:** `*text*` or `_text_`
- **Bold + Italic:** `***text***`
- **Links:** `[text](url)`
- **Images:** `![alt](image.png)`
- **Code:** `` `inline` `` and ` ```blocks``` `
- **Lists:** `- unordered`, `1. ordered`
- **Blockquotes:** `> quote`
- **Horizontal rules:** `---` or `***`

### Obsidian Extensions

- **Callouts:** `> [!note]`, `> [!warning]`, `> [!tip]` (20+ types)
- **Wikilinks:** `[[Note]]`, `[[Note|Display Text]]`
- **Highlights:** `==marked text==`
- **Comments:** `%%hidden comment%%` (removed from output)
- **Task lists:** `- [ ] todo`, `- [x] done`
- **Image sizing:** `![[image.png|300]]` (size stripped for HTML)

See complete syntax guide: [references/supported-markdown.md](references/supported-markdown.md)

## Platform-Specific Notes

### Substack

**Best compatibility:**
- Paste into rich text editor
- Images load from public URLs
- Formatting preserved
- Callouts render as styled boxes

**Known issues:**
- None—full support

### Medium

**Best compatibility:**
- Paste into editor
- Images load automatically
- Most formatting preserved

**Known issues:**
- Custom callout styling may simplify
- Task list checkboxes become plain bullets

**Workaround:** Use bold for callouts instead of colored boxes.

### LinkedIn

**Best compatibility:**
- Paste into article editor (not posts)
- Images load from URLs
- Basic formatting preserved

**Known issues:**
- Callouts lose custom styling
- Code blocks render as plain text
- Task lists become plain bullets

**Workaround:** LinkedIn is best for text-heavy articles without complex formatting.

## Error Handling

### Article not found in blog

```
❌ Article not found: [search-term]

Searched: [blog-path]/content/articles/

Available articles:
  - article-one
  - article-two

Did you mean one of these?

Or: Have you published this article yet?
Run /article-to-blog first.
```

### Blog repository not found

```
❌ Blog repository not found at: [path]

Check:
1. Path is correct: ls [path]
2. It's a Quartz blog: ls [path]/content
3. Articles exist: ls [path]/content/articles

Need to set up a blog? Use /article-to-blog for guidance.
```

### No images found

```
⚠️  No images found in article folder

Checked: [blog-path]/content/articles/[slug]/

HTML will be generated without images.
Continue?
```

### GitHub Pages URL not set

```
❌ GitHub Pages URL required for image links

Example: https://username.github.io/blog

This is where your blog is deployed.

Find it:
1. GitHub repo → Settings → Pages
2. Look for "Your site is live at: [URL]"

What's your GitHub Pages URL?
```

## Related Skills

- `/article-to-blog` - Publish articles to your blog (must run first)
- `/api-store` - Store GitHub credentials for private repos

## Notes

**Requires article published first:**
- This skill reads from your blog repo
- Article must exist in `content/articles/[slug]/`
- Run `/article-to-blog` before this skill

**Image hosting:**
- All images loaded from your GitHub Pages blog
- Must be publicly accessible
- No image upload to Substack/Medium needed

**HTML is temporary:**
- Saved to `/tmp/` (auto-cleaned by OS)
- Only used for copy-paste
- Original markdown stays in blog repo

**Manual workflow:**
- No API calls to Substack/Medium/LinkedIn
- You control when to publish
- Allows editing after paste
- Platform-specific adjustments possible
