---
name: article-to-blog
description: Publish markdown articles with images to GitHub Pages using Quartz v4. Guides through blog setup, article organization, and automated deployment.
user-invocable: true
disable-model-invocation: false
---

# Article to Blog Publisher

Publish markdown articles with images to your GitHub Pages blog using Quartz v4.

## First-Time Setup

If you don't have a Quartz v4 blog yet, this skill will guide you through:

1. **Installing Quartz v4** - Interactive setup for static site generator
2. **GitHub Pages configuration** - Deploy your blog
3. **Repository structure** - Where articles and images live
4. **Article organization** - Your preferred file structure

See [references/quartz-v4-setup.md](references/quartz-v4-setup.md) for detailed setup instructions.

## When to Use This Skill

- You've finished writing an article in markdown
- Article includes images in an attachments folder
- You want to publish it to your GitHub Pages blog
- You want automated git deployment

## Workflow

### Step 1: Check for Quartz Blog

Ask: "Do you have a Quartz v4 blog set up with GitHub Pages?"

**If NO:**
- Guide through Quartz v4 installation (see references/quartz-v4-setup.md)
- Help set up GitHub repository
- Configure GitHub Pages deployment
- Return to this workflow when complete

**If YES:**
- Continue to Step 2

### Step 2: Locate Blog Repository

Ask: "Where is your blog repository located?"

**Example:** `/Users/username/Documents/blog` or `~/projects/my-blog`

**Verify:**
```bash
ls [blog-path]/content
```

If `content/` directory doesn't exist:
```
❌ This doesn't appear to be a Quartz v4 repository.

Expected structure:
blog/
├── content/
├── quartz.config.ts
└── quartz.layout.ts

Run: ls [blog-path]

Would you like guidance on setting up Quartz v4?
```

**If valid:** Store path for this session, continue to Step 3.

### Step 3: Locate Article Source

Ask: "Where are your completed articles stored?"

**Example:** `/Users/username/Documents/articles/done` or `~/writing/published`

**Show file organization options:**

1. **Flat structure** - All articles in one folder
   ```
   articles/done/
   ├── Article_Name_One/
   └── Article_Name_Two/
   ```

2. **Brand/category organized** - Grouped by topic/brand
   ```
   articles/
   ├── personal/done/
   ├── business/done/
   └── tech/done/
   ```

3. **Date-based** - Organized chronologically
   ```
   articles/done/
   ├── 2026-02/
   └── 2026-01/
   ```

Ask: "Which structure are you using, or where should I look for your article?"

See [references/file-organization.md](references/file-organization.md) for detailed organization patterns.

**If user has custom structure:**
- Ask them to specify the exact path to the article folder
- Continue to Step 4

### Step 4: Select Article to Publish

**If directory provided:**
List available articles:
```bash
ls [article-source-path]
```

Show user the available article folders and ask: "Which article do you want to publish?"

**If specific article folder provided:**
- Verify it exists
- Check for markdown file and attachments
- Continue to Step 5

### Step 5: Verify Article Structure

Expected structure:
```
Article_Folder_Name/
├── Article_Folder_Name.md
└── attachments/
    ├── image1.png
    └── image2.png
```

**Check:**
1. Markdown file exists with same name as folder
2. Images in `attachments/` subfolder (if any)

**If structure doesn't match:**
Ask: "Your article structure looks different. Can you confirm:
- Path to the markdown file
- Path to images folder (if separate)"

### Step 6: Generate URL Slug

From folder name: `Downloaded_My_Brain_Into_Claude_Code`
To slug: `downloaded-my-brain-into-claude-code`

**Rules:**
- Lowercase
- Replace underscores/spaces with hyphens
- Remove special characters
- Keep alphanumeric and hyphens only

Show user: "Generated slug: `[slug]`"

Ask: "Use this slug, or provide a custom one?"

### Step 7: Check for END Marker

Read markdown file and check for `# END` marker.

**If found:**
```
✂️  Found # END marker
- Will publish content BEFORE the marker only
- Private notes after # END will not be published
- Continue?
```

**If not found:**
```
⚠️  No # END marker found
- Entire article will be published
- Add "# END" to your markdown to exclude private notes
- Continue anyway?
```

See workflow detail: This lets users keep private notes/drafts in the same file.

### Step 8: Copy to Blog Repository

**Create blog article folder:**
```
[blog-path]/content/articles/[slug]/
```

**Copy files:**
1. `Article_Name.md` → `index.md` (truncated at # END if present)
2. All images from `attachments/` → `[slug]/` folder
3. Sanitize image filenames (spaces → hyphens)
4. Only copy images referenced in published content (before # END)

**Show progress:**
```
✓ Created: content/articles/[slug]/
✓ Wrote: index.md ([lines] lines)
✓ Copied: image1.png
✓ Copied: image2.png
📸 Images: 2 copied, 0 skipped
```

### Step 9: Configure Git Author

Ask: "What name should appear in git commits?"

Ask: "What email should appear in git commits?"

**Example:**
- Name: `Your Name`
- Email: `you@example.com`

**Set locally for blog repo only:**
```bash
git -C [blog-path] config user.name "[name]"
git -C [blog-path] config user.email "[email]"
```

**Note:** This only affects the blog repository, not your global git config.

### Step 10: Commit and Push

**Git workflow:**
```bash
cd [blog-path]
git add content/articles/[slug]/
git commit -m "feat: add article about [title]

Article: [title]
Slug: [slug]
Published: [date]

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>"

git push
```

**Show status:**
```
✓ Committed to git
✓ Pushed to GitHub
⏳ GitHub Actions deploying...
```

### Step 11: Return Blog URL

Ask: "What's your GitHub Pages URL?"

**Example:** `https://username.github.io/blog`

**Generate article URL:**
```
✅ Article published!

🌐 URL: https://username.github.io/blog/articles/[slug]/
📸 Images: https://username.github.io/blog/articles/[slug]/[image.png]

⏳ GitHub Actions will deploy in ~1-2 minutes
Check deployment: https://github.com/[username]/[repo]/actions
```

## Error Handling

### Blog repository not found
```
❌ Blog repository not found at: [path]

Try:
1. Check the path is correct
2. Navigate there: cd [path]
3. Verify it's a Quartz blog: ls content/

Need help setting up Quartz v4? I can guide you through it.
```

### Article folder not found
```
❌ Article not found at: [path]

Try:
1. List your articles: ls [article-source-path]
2. Provide the complete path to the article folder
3. Verify folder name matches markdown filename
```

### Article already exists in blog
```
❌ Article already exists in blog

Path: content/articles/[slug]/

Options:
1. Remove existing: rm -rf content/articles/[slug]/
2. Use different slug
3. Update existing article (will overwrite)

Which would you like to do?
```

### Git push fails
```
❌ Git push failed

Common causes:
1. No remote configured: git remote -v
2. Authentication required: gh auth login
3. Conflicts: git pull --rebase

Error details:
[git error output]

Would you like help troubleshooting?
```

## Related Skills

- `/article-to-html` - Convert published articles to HTML for Substack/Medium/LinkedIn
- `/api-store` - Store GitHub credentials if using private repositories

## Notes

**This is an atomic skill:**
- Does ONE thing: publish article to blog
- Can be used standalone or orchestrated by other skills
- All prompts are interactive (no config file required)

**Security:**
- Git author info stored locally in blog repo only
- No credentials stored (uses system git auth)
- Private notes after # END marker not published

**Publishing method:**
- Git commit + push triggers GitHub Actions
- Quartz v4 automatically rebuilds
- Deploys to GitHub Pages
- No API calls needed
