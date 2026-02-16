---
name: api-store
description: Guide users to store API keys securely in .env files, prevent git leaks via .gitignore, and optionally create skills that use those keys. Use when the user mentions API keys, wants to integrate with external services, asks about storing credentials, or when you notice them sharing sensitive keys in chat.
user-invocable: true
disable-model-invocation: false
---

# API Key Storage Guide

Guides users through secure API key storage in `.env` files with git protection and optional skill creation.

## When to Use This Skill

- User mentions needing to store API keys
- User wants to integrate with external services (Notion, Typefully, etc.)
- User asks about credential management
- **CRITICAL:** User has pasted an API key in chat (immediate intervention)
- User mentions storing keys in `.zshrc` or shell config
- User is creating a new skill that needs API access

## Workflow

### Step 1: Check if User Has API Key

Ask: "Do you already have an API key for [service]?"

**If NO:**
- Direct them to service's API documentation
- Provide direct link if known (e.g., https://developers.notion.com for Notion)
- Wait for them to obtain key before continuing

**If YES:**
- Continue to Step 2

### Step 2: Security Check - Key Exposure

**If user has pasted API key in chat:**

```
⚠️ SECURITY WARNING ⚠️

You've shared your API key in this chat. This is NOT secure.

Chat logs may be stored or transmitted. This key is now compromised.

REQUIRED ACTION:
1. Revoke this key immediately from [service] dashboard
2. Generate a new API key
3. Do NOT paste the new key in chat

Once you have a new key, continue with this setup.
```

**If key not shared:**
- Proceed to Step 3

### Step 3: Multi-Account Setup

Ask: "Are you using multiple accounts for [service], or just one?"

**Single account:**
- Variable format: `SERVICE_API_KEY`
- Example: `NOTION_API_KEY`, `TYPEFULLY_API_KEY`

**Multiple accounts:**
- Variable format: `SERVICE_API_KEY_ACCOUNTNAME`
- Examples: `NOTION_API_KEY_PERSONAL`, `TYPEFULLY_API_KEY_FIP`, `TYPEFULLY_API_KEY_AUTOMATA`

**If multiple accounts:**
1. Ask: "How many accounts do you want to configure?"
2. Ask: "What should we call each account?" (e.g., personal, work, client1, nuhgid, FIP)
3. Create variables for each account

**For common services**, see [references/common-services.md](references/common-services.md) for recommended naming conventions.

### Step 4: Create or Update .env File

**Check if `.env` exists at repo root:**

**If .env doesn't exist:**
1. Create `.env` at repository root
2. Add header comment:
```bash
# API Keys and Secrets
# DO NOT commit this file to git
# Add .env to .gitignore before adding any keys
```

**If .env exists:**
1. Read current contents
2. Append new entry (don't overwrite)

**Add API key template:**

```bash
# [Service Name] API Key ([Account Name])
SERVICE_API_KEY_ACCOUNTNAME=YOUR_KEY_HERE
```

**Show user:**
```
I've added a template to your .env file:

SERVICE_API_KEY_ACCOUNTNAME=YOUR_KEY_HERE

IMPORTANT:
1. Open .env in your editor
2. Replace YOUR_KEY_HERE with your actual API key
3. DO NOT paste the key in this chat
4. Save the file

When done, type "ready" to continue.
```

Wait for user confirmation.

### Step 5: Update .gitignore

**Check if `.gitignore` exists:**

**If missing:**
- Create `.gitignore` at repo root

**Verify comprehensive .env protection:**

Required patterns:
```gitignore
# Environment files and secrets
.env
.env.*
!.env.*.template  # Allow template files only
*.key
*.pem
credentials.json
token.json
*secret*
*SECRET*
```

**If patterns missing:**
1. Add missing patterns to `.gitignore`
2. Explain what each pattern protects

**Confirm:**
```
✓ .gitignore updated to protect .env files and secrets
```

### Step 6: Set File Permissions

**For Unix/Mac systems:**

Recommend running:
```bash
chmod 600 .env
```

**Explain:**
- `600` = Only you can read/write
- Default `644` allows others to read your secrets
- This prevents accidental exposure on shared systems

**For Windows:**
- File permissions less critical
- Focus on .gitignore protection

### Step 7: Offer Skill Creation

Ask: "Would you like to create a Claude Code skill that uses this API key?"

**If YES:**

1. Ask: "What should we name this skill?" (e.g., `notion-sync`, `typefully-publish`)
2. Ask: "What will this skill do?" (brief description)

3. Create skill structure:
```
.claude/skills/[skill-name]/
└── SKILL.md
```

4. Generate SKILL.md with:

```markdown
---
name: [skill-name]
description: [user's description]
user-invocable: true
disable-model-invocation: false
---

# [Skill Title]

[Skill description]

## Setup

This skill requires the [Service] API key.

**Environment variable:** `SERVICE_API_KEY_ACCOUNTNAME`

### Configuration

Add to your `.env` file (do NOT commit):

```bash
# [Service] API Key
SERVICE_API_KEY_ACCOUNTNAME=your_actual_key_here
```

### Loading in Python

```python
import os
from dotenv import load_dotenv

load_dotenv()

api_key = os.environ.get("SERVICE_API_KEY_ACCOUNTNAME")
if not api_key:
    raise ValueError("SERVICE_API_KEY_ACCOUNTNAME not found in environment")
```

### Loading in JavaScript/Node.js

```javascript
require('dotenv').config();

const apiKey = process.env.SERVICE_API_KEY_ACCOUNTNAME;
if (!apiKey) {
  throw new Error('SERVICE_API_KEY_ACCOUNTNAME not found in environment');
}
```

## Security Reminders

- Never commit `.env` to git
- Never paste API keys in chat or logs
- Regenerate keys if accidentally exposed
- Use `.env.template` for sharing configuration examples (without actual keys)
```

5. Show user:
```
✓ Skill created: .claude/skills/[skill-name]/SKILL.md

The skill template includes:
- Environment variable reference
- Loading examples (Python & JavaScript)
- Security reminders

You can now customize the skill implementation.
```

**If NO:**
- Skip skill creation
- Continue to Step 8

### Step 8: Discourage .zshrc Usage

**If user mentions `.zshrc`, `~/.bashrc`, or shell config:**

Show comparison:

```
❌ Shell config approach (~/.zshrc, ~/.bashrc):

- Global to ALL projects on your system
- Mixed with shell configuration
- Hard to audit which project uses which key
- No automatic git protection
- Keys exposed to all terminal processes
- Difficult to rotate per-project

✅ .env file approach (recommended):

- Project-specific secrets
- Separate from code and config
- Easy to audit and rotate
- Automatically ignored by git (.gitignore)
- Standard across languages (dotenv libraries exist everywhere)
- Only loaded when explicitly needed
- Different keys per project/environment
```

**Explain:**

"While `.zshrc` works for personal projects, `.env` files are industry best practice because:

1. **Isolation**: Each project has its own secrets
2. **Security**: .gitignore automatically protects them
3. **Portability**: Works across languages (Python, Node, Ruby, etc.)
4. **Team-friendly**: Other developers can use their own keys
5. **Environment-specific**: Dev/staging/prod keys stay separate

For personal projects on your local machine, `.zshrc` won't cause immediate problems, but `.env` builds better habits and works better as projects grow."

**Don't block .zshrc usage**, but strongly recommend .env approach.

### Step 9: Verification Checklist

Show final checklist:

```
✅ API Key Setup Complete!

Service: [Service Name]
Account: [Account Name or "default"]
Variable: SERVICE_API_KEY_ACCOUNTNAME

Security checklist:
✓ API key stored in .env (not in chat)
✓ .env added to .gitignore
✓ File permissions set (chmod 600 .env) [Unix/Mac only]
✓ Variable name documented
✓ [If applicable] Skill created with API integration

Final verification:
```

Ask: "Would you like me to verify your .gitignore is protecting .env?"

**If YES:**

Run verification:
```bash
git ls-files .env
```

**Expected output:** (empty - no output)

If `.env` appears in output:
```
⚠️ WARNING: .env is tracked by git!

This means it will be committed and pushed to GitHub.

Fix this now:
1. git rm --cached .env
2. Verify .gitignore includes .env
3. git add .gitignore
4. git commit -m "security: remove .env from git tracking"

Then verify again: git ls-files .env (should be empty)
```

If no output:
```
✓ Confirmed: .env is NOT tracked by git
Your API keys are protected!
```

## When NOT to Use This Skill

**Don't use this skill for:**

1. **OAuth credentials** (different workflow)
   - Use `/google-workspace-auth` for Google services
   - OAuth uses token exchange, not simple API keys

2. **Database credentials** (consider stronger security)
   - Production databases: Use secrets manager (AWS Secrets Manager, HashiCorp Vault)
   - Development databases: .env is acceptable

3. **User hasn't obtained API key yet**
   - Direct them to service documentation first
   - Come back to this skill once they have the key

4. **SSH keys or certificates**
   - Different storage and permission requirements
   - Use `~/.ssh/` with proper permissions

## Error Handling

### User Shared API Key in Chat

**Immediate action:**
1. Stop the workflow
2. Show security warning
3. Recommend key regeneration
4. Explain why it's compromised
5. Wait for confirmation before continuing

### .env Already Exists

**Safe handling:**
1. Read current contents
2. Append new key (preserve existing entries)
3. Maintain formatting and comments
4. Don't overwrite or delete existing keys

### .gitignore Missing

**Create it:**
1. Generate new `.gitignore`
2. Add comprehensive secret patterns
3. Explain what each pattern protects
4. Verify with `git ls-files .env`

### User Insists on .zshrc

**Educate, don't block:**
1. Show comparison (.env vs .zshrc)
2. Explain trade-offs clearly
3. Recommend .env for best practices
4. Allow them to proceed if they understand risks

### Variable Name Conflicts

**If variable already exists in .env:**
1. Check if it's the same service
2. Ask if they want to replace or add additional account
3. Use numbered suffix: `SERVICE_API_KEY_2`, `SERVICE_API_KEY_3`

## Security Principles

Based on industry best practices and real-world mistakes:

1. **Single source of truth** - One .env file, not duplicated across shell configs
2. **Variable names must match** - Skills/scripts expect exact names
3. **Test immediately** - Run `echo $VAR_NAME` to verify loading
4. **File permissions matter** - `chmod 600` on Unix/Mac systems
5. **Comprehensive .gitignore** - Use wildcards (`.env.*`) to catch variants
6. **Exposed keys must be revoked** - Git history is permanent
7. **Never commit secrets** - Check with `git ls-files .env` before committing
8. **Use templates for sharing** - `.env.template` with placeholder values

## Output Summary Template

After successful completion:

```
✅ API Key Setup Complete!

Service: [Service Name]
Account: [Account Name]
Variable: SERVICE_API_KEY_ACCOUNTNAME

Location: .env (NOT committed to git)
Protected by: .gitignore patterns
Permissions: 600 (owner read/write only)

Next steps:
1. Open .env and replace YOUR_KEY_HERE with your actual key
2. Verify protection: git ls-files .env (should be empty)
3. Test loading: echo $SERVICE_API_KEY_ACCOUNTNAME (after sourcing)
4. [If skill created] Customize: .claude/skills/[skill-name]/SKILL.md

Security reminders:
- Never commit .env to git
- Regenerate keys if exposed
- Use different keys for dev/staging/prod
- Rotate keys periodically
```

## Related Skills

- `/google-workspace-auth` - OAuth credential setup for Google services (different workflow)
- `/skill-creator` - Create custom skills (used when generating API-integrated skills)

## Notes

**Progressive disclosure approach:**
- Ask questions step-by-step (don't overwhelm)
- Show examples at each stage
- Verify before moving to next step

**Security-first mindset:**
- Warn about exposed keys IMMEDIATELY
- Verify .gitignore before adding keys
- Always recommend regeneration if compromised

**Automation where helpful:**
- Create/update .env automatically
- Update .gitignore automatically
- Generate skill templates

**User verification:**
- Help user confirm everything is secure
- Show them how to check git tracking
- Provide testing commands
