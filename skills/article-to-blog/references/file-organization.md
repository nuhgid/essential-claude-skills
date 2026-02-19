# Article File Organization Patterns

Different ways to organize your article source files before publishing to blog.

## Pattern 1: Flat Structure

**Best for:** Solo writers, single-topic blogs, simple workflows

```
articles/
├── done/
│   ├── My_First_Article/
│   │   ├── My_First_Article.md
│   │   └── attachments/
│   │       └── image.png
│   └── Another_Article/
│       ├── Another_Article.md
│       └── attachments/
│           └── chart.png
└── drafts/
    └── Work_In_Progress/
```

**Pros:**
- Simple, easy to find articles
- No category decisions
- Fast setup

**Cons:**
- Gets messy with 50+ articles
- No organization by topic

---

## Pattern 2: Category/Brand Organized

**Best for:** Multiple brands, different topics, content creators

```
articles/
├── personal/
│   ├── done/
│   │   └── Life_Update/
│   └── drafts/
├── tech/
│   ├── done/
│   │   └── How_To_Code/
│   └── drafts/
└── business/
    ├── done/
    │   └── Marketing_Tips/
    └── drafts/
```

**Pros:**
- Clear separation by topic
- Easy to find articles by category
- Scales well

**Cons:**
- Need to decide categories upfront
- Moving between categories requires reorganization

---

## Pattern 3: Date-Based

**Best for:** Time-sensitive content, journals, regular publishers

```
articles/
└── done/
    ├── 2026-02/
    │   ├── Article_One/
    │   └── Article_Two/
    ├── 2026-01/
    │   └── January_Post/
    └── 2025-12/
        └── Year_End_Review/
```

**Pros:**
- Chronological organization
- Easy to find recent articles
- Natural archiving

**Cons:**
- Hard to find articles by topic
- Date in folder structure is redundant (already in frontmatter)

---

## Pattern 4: Status Pipeline

**Best for:** Editorial workflows, team collaboration, content pipelines

```
articles/
├── ideas/
│   └── Article_Idea.md (single file)
├── drafts/
│   └── First_Draft/
│       ├── First_Draft.md
│       └── attachments/
├── review/
│   └── Ready_For_Review/
├── approved/
│   └── Approved_Article/
└── done/
    └── Published_Article/
```

**Pros:**
- Clear workflow stages
- Easy to see what needs attention
- Works well for teams

**Cons:**
- More complex
- Requires moving folders between stages

---

## Recommended Structure

**For `/article-to-blog` skill:**

```
[Your Articles Root]/
└── done/
    └── Article_Folder_Name/
        ├── Article_Folder_Name.md
        └── attachments/
            ├── image1.png
            └── image2.png
```

**Requirements:**
1. All publishable articles in a `done/` folder (or equivalent)
2. Each article in its own folder
3. Markdown file named same as folder
4. Images in `attachments/` subfolder

**Folder naming:**
- Use `Sentence_Case_With_Underscores`
- Example: `How_To_Build_With_Claude`
- Becomes slug: `how-to-build-with-claude`

**Markdown filename:**
- Must match folder name exactly
- Example: `How_To_Build_With_Claude/How_To_Build_With_Claude.md`

**Images:**
- All images in `attachments/` subfolder
- Any format: PNG, JPG, GIF, WebP
- Names can have spaces (will be sanitized to hyphens)

---

## Private Notes with # END Marker

Keep private notes in the same file using `# END` marker:

```markdown
---
title: My Article
---

# My Article

This content will be published.

## Section One

Public content here.

# END

## Private Notes

These notes won't be published to the blog.

- Todo: Add more examples
- Research: Find better statistics
- Draft ideas for follow-up article
```

**How it works:**
- Everything BEFORE `# END` → published to blog
- Everything AFTER `# END` → kept private
- Lets you keep drafts, notes, and final version in one file

**When publishing:**
```
✂️  Truncated at # END marker:
   Original: 150 lines
   Published: 87 lines
   Excluded: 63 lines
```

---

## Migration from Existing Structure

**If you have a different structure:**

The `/article-to-blog` skill will ask you to specify:
1. Path to the article folder
2. Path to the markdown file
3. Path to images folder

**Example conversation:**
```
Claude: "Where is your article located?"
You: "~/Documents/writing/2026/february/my-article/"

Claude: "I see the folder. Where is the markdown file?"
You: "~/Documents/writing/2026/february/my-article/article.md"

Claude: "Where are the images?"
You: "~/Documents/writing/2026/february/my-article/images/"

Claude: ✓ Found article
✓ Found 3 images
Ready to publish!
```

No need to reorganize—just tell the skill where things are.
