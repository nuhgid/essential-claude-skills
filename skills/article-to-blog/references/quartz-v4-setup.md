# Setting Up Quartz v4 with GitHub Pages

Complete guide to installing Quartz v4 and deploying to GitHub Pages.

## What is Quartz v4?

Quartz is a fast static site generator optimized for publishing notes and articles. It converts markdown to a beautiful, searchable website.

**Benefits:**
- Write in markdown (or Obsidian)
- Git-based workflow
- Free hosting on GitHub Pages
- Automatic deployments
- Full-text search
- Wikilinks support
- Fast builds

## Prerequisites

1. **Node.js installed** (v18.14 or higher)
   ```bash
   node --version
   ```
   If not installed: https://nodejs.org/

2. **Git installed**
   ```bash
   git --version
   ```

3. **GitHub account**
   - Create at: https://github.com/signup
   - Set up authentication: `gh auth login`

## Installation Steps

### Step 1: Fork Quartz Repository

```bash
# Navigate to where you want your blog
cd ~/Documents

# Clone Quartz v4
git clone https://github.com/jackyzha0/quartz.git blog
cd blog

# Or use custom name
git clone https://github.com/jackyzha0/quartz.git my-blog-name
cd my-blog-name
```

### Step 2: Install Dependencies

```bash
npm install
```

This installs all Quartz dependencies (~2-3 minutes).

### Step 3: Create GitHub Repository

**Option A: Using GitHub CLI**
```bash
gh repo create blog --public --source=. --remote=origin --push
```

**Option B: Manual**
1. Go to https://github.com/new
2. Repository name: `blog` (or your preferred name)
3. Public repository
4. Don't initialize with README (you already have one)
5. Create repository

Then push:
```bash
git remote add origin https://github.com/[username]/blog.git
git push -u origin v4
```

### Step 4: Configure GitHub Pages

1. Go to repository settings: `https://github.com/[username]/blog/settings/pages`
2. Source: **GitHub Actions**
3. Save

GitHub will now auto-deploy when you push to the `v4` branch.

### Step 5: Configure Quartz

Edit `quartz.config.ts`:

```typescript
const config: QuartzConfig = {
  configuration: {
    pageTitle: "Your Blog Name",
    enableSPA: true,
    enablePopovers: true,
    analytics: {
      provider: "plausible", // optional
    },
    baseUrl: "username.github.io/blog", // IMPORTANT: Change this!
    ignorePatterns: ["private", "templates", ".obsidian"],
    theme: {
      typography: {
        header: "Schibsted Grotesk",
        body: "Source Sans Pro",
        code: "IBM Plex Mono",
      },
      colors: {
        lightMode: {
          light: "#faf8f8",
          lightgray: "#e5e5e5",
          gray: "#b8b8b8",
          darkgray: "#4e4e4e",
          dark: "#2b2b2b",
          secondary: "#284b63",
          tertiary: "#84a59d",
          highlight: "rgba(143, 159, 169, 0.15)",
        },
        darkMode: {
          light: "#161618",
          lightgray: "#393639",
          gray: "#646464",
          darkgray: "#d4d4d4",
          dark: "#ebebec",
          secondary: "#7b97aa",
          tertiary: "#84a59d",
          highlight: "rgba(143, 159, 169, 0.15)",
        },
      },
    },
  },
}
```

**Important:** Change `baseUrl` to your GitHub Pages URL!

### Step 6: Create Content Structure

```bash
mkdir -p content/articles
```

Quartz reads from `content/` directory by default.

**Structure:**
```
blog/
├── content/
│   ├── articles/          # Your blog articles
│   ├── index.md           # Homepage
│   └── about.md           # About page (optional)
├── quartz/                # Quartz engine (don't modify)
├── quartz.config.ts       # Your config
├── quartz.layout.ts       # Layout config
└── package.json
```

### Step 7: Test Locally

```bash
npx quartz build --serve
```

Open http://localhost:8080 to preview your blog.

**Hot reload:** Changes to markdown files auto-refresh.

### Step 8: Deploy to GitHub Pages

```bash
git add .
git commit -m "Initial Quartz setup"
git push
```

**GitHub Actions will:**
1. Build your site
2. Deploy to GitHub Pages
3. Available at: `https://[username].github.io/blog/`

Check deployment status:
```bash
gh run list
```

Or visit: `https://github.com/[username]/blog/actions`

## Creating Your First Article

```bash
# Create article folder
mkdir -p content/articles/my-first-post

# Create markdown file
cat > content/articles/my-first-post/index.md << 'EOF'
---
title: My First Post
date: 2026-02-19
---

# My First Post

This is my first article using Quartz v4!

## Features

- Fast builds
- Beautiful design
- Full-text search
- Wikilinks support

![Example Image](image.png)
EOF

# Add an image (optional)
cp ~/path/to/image.png content/articles/my-first-post/
```

**Commit and deploy:**
```bash
git add content/articles/my-first-post/
git commit -m "feat: add my first post"
git push
```

Wait ~1-2 minutes, then visit:
```
https://[username].github.io/blog/articles/my-first-post/
```

## Troubleshooting

### Build fails with "Cannot find module"
```bash
rm -rf node_modules package-lock.json
npm install
```

### GitHub Pages shows 404
1. Check Settings → Pages → Source = "GitHub Actions"
2. Verify `baseUrl` in `quartz.config.ts` matches your repo
3. Check Actions tab for deployment errors

### Images don't load
- Verify image is in same folder as `index.md`
- Use relative paths: `![alt](image.png)` not `![alt](/path/to/image.png)`
- Check file extensions match (case-sensitive)

### Changes don't appear
- Allow 1-2 minutes for GitHub Actions
- Check Actions tab: https://github.com/[username]/blog/actions
- Hard refresh: Cmd+Shift+R (Mac) or Ctrl+F5 (Windows)

## Quartz Documentation

- Official docs: https://quartz.jzhao.xyz/
- Configuration: https://quartz.jzhao.xyz/configuration
- Layouts: https://quartz.jzhao.xyz/layout
- Plugins: https://quartz.jzhao.xyz/plugins

## Next Steps

Once Quartz is set up, use `/article-to-blog` to automatically publish articles to your blog!
