# Common API Services - Variable Naming Guide

Quick reference for popular API services and their recommended environment variable names.

## Content & Productivity

### Notion
**Single account:**
```bash
NOTION_API_KEY=your_key_here
```

**Multiple accounts:**
```bash
NOTION_API_KEY_PERSONAL=your_key_here
NOTION_API_KEY_WORK=your_key_here
NOTION_API_KEY_CLIENT=your_key_here
```

**Where to get:**
- Go to https://www.notion.so/my-integrations
- Create new integration
- Copy "Internal Integration Token"

### Typefully
**Single account:**
```bash
TYPEFULLY_API_KEY=your_key_here
```

**Multiple accounts:**
```bash
TYPEFULLY_API_KEY_PERSONAL=your_key_here
TYPEFULLY_API_KEY_BUSINESS=your_key_here
```

**Where to get:**
- Settings → API → Generate API Key

### Airtable
**Single account:**
```bash
AIRTABLE_API_KEY=your_key_here
AIRTABLE_BASE_ID=your_base_id_here
```

**Multiple accounts:**
```bash
AIRTABLE_API_KEY_PERSONAL=your_key_here
AIRTABLE_API_KEY_WORK=your_key_here
```

**Where to get:**
- Account → Generate API Key
- Base ID from URL: `airtable.com/BASE_ID/table_name`

## Developer Tools

### GitHub
**Personal Access Token:**
```bash
GITHUB_TOKEN=your_token_here
GITHUB_USERNAME=your_username
```

**Where to get:**
- Settings → Developer settings → Personal access tokens → Tokens (classic)
- Generate new token with required scopes

### OpenAI
**Single account:**
```bash
OPENAI_API_KEY=your_key_here
```

**Where to get:**
- https://platform.openai.com/api-keys
- Create new secret key

### Anthropic (Claude)
**Single account:**
```bash
ANTHROPIC_API_KEY=your_key_here
```

**Where to get:**
- https://console.anthropic.com/settings/keys
- Create API Key

## Communication

### Slack
**Workspace Bot:**
```bash
SLACK_BOT_TOKEN=xoxb-your-token
SLACK_SIGNING_SECRET=your_secret_here
```

**Multiple workspaces:**
```bash
SLACK_BOT_TOKEN_PERSONAL=xoxb-token1
SLACK_BOT_TOKEN_WORK=xoxb-token2
```

**Where to get:**
- https://api.slack.com/apps
- Create app → OAuth & Permissions → Bot User OAuth Token

### Discord
**Bot Token:**
```bash
DISCORD_BOT_TOKEN=your_token_here
DISCORD_APPLICATION_ID=your_app_id
```

**Where to get:**
- https://discord.com/developers/applications
- Create application → Bot → Reset Token

### Microsoft Teams
**App Registration:**
```bash
TEAMS_CLIENT_ID=your_client_id
TEAMS_CLIENT_SECRET=your_secret
TEAMS_TENANT_ID=your_tenant_id
```

**Where to get:**
- Azure Portal → App registrations
- Register application
- Certificates & secrets → New client secret

## Data & Analytics

### Google Analytics
**Measurement API:**
```bash
GOOGLE_ANALYTICS_MEASUREMENT_ID=G-XXXXXXXXXX
GOOGLE_ANALYTICS_API_SECRET=your_secret
```

**Where to get:**
- Admin → Data Streams → Measurement Protocol API secrets

### Stripe
**Payment processing:**
```bash
STRIPE_SECRET_KEY=sk_test_your_key  # Test mode
STRIPE_PUBLISHABLE_KEY=pk_test_your_key
```

**Production:**
```bash
STRIPE_SECRET_KEY=sk_live_your_key  # Live mode
STRIPE_PUBLISHABLE_KEY=pk_live_your_key
```

**Where to get:**
- Dashboard → Developers → API keys

## Typing & Practice

### Monkeytype
**Personal API:**
```bash
MONKEYTYPE_API_KEY=your_key_here
```

**Where to get:**
- Settings → API → Generate Token

## Search & Web

### Brave Search
**API Key:**
```bash
BRAVE_API_KEY=your_key_here
```

**Where to get:**
- https://brave.com/search/api/
- Sign up for API access

### Google Custom Search
**Search API:**
```bash
GOOGLE_SEARCH_API_KEY=your_key_here
GOOGLE_SEARCH_ENGINE_ID=your_cx_id
```

**Where to get:**
- Google Cloud Console → APIs & Services
- Custom Search JSON API

## Database Services

### Supabase
**Project credentials:**
```bash
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_ANON_KEY=your_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_service_key  # Keep secret!
```

**Where to get:**
- Project Settings → API

### MongoDB Atlas
**Connection string:**
```bash
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/database
```

**Where to get:**
- Cluster → Connect → Connect your application

## Naming Conventions

### General Pattern

**Single account:**
```bash
SERVICE_API_KEY=value
```

**Multiple accounts:**
```bash
SERVICE_API_KEY_ACCOUNTNAME=value
```

**Multiple credential types:**
```bash
SERVICE_CLIENT_ID=value
SERVICE_CLIENT_SECRET=value
SERVICE_API_KEY=value
```

### Account Naming Examples

**By purpose:**
- `_PERSONAL`, `_WORK`, `_CLIENT`
- `_PRODUCTION`, `_STAGING`, `_DEVELOPMENT`
- `_PRIMARY`, `_BACKUP`

**By brand/project:**
- `_NUHGID`, `_FIP`, `_AUTOMATA`
- `_PROJECTA`, `_PROJECTB`

**By number (if no clear name):**
- `_ACCOUNT1`, `_ACCOUNT2`, `_ACCOUNT3`

### Best Practices

1. **Uppercase only** - `SERVICE_API_KEY`, not `Service_Api_Key`
2. **Underscores for spaces** - `GOOGLE_CLOUD_API_KEY`
3. **Descriptive suffixes** - `_PERSONAL` vs `_WORK`, not `_1` vs `_2`
4. **Consistent ordering** - Service name first, then credential type, then account
5. **Avoid abbreviations** - `GOOGLE` not `GOOG`, `NOTION` not `NTN`

## Special Cases

### OAuth Credentials (Not Simple API Keys)

**Google OAuth:**
```bash
GOOGLE_CLIENT_ID=your_client_id.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=GOCSPX-your_secret
```

**Note:** OAuth requires different setup. Use `/google-workspace-auth` skill for Google services.

### Multiple Credential Types

**Microsoft Azure:**
```bash
AZURE_TENANT_ID=your_tenant_id
AZURE_CLIENT_ID=your_client_id
AZURE_CLIENT_SECRET=your_secret
```

**AWS:**
```bash
AWS_ACCESS_KEY_ID=AKIA...
AWS_SECRET_ACCESS_KEY=your_secret
AWS_DEFAULT_REGION=us-east-1
```

### Service-Specific Patterns

Some services have established conventions:

**Anthropic:**
- `ANTHROPIC_API_KEY` (official SDK uses this)

**OpenAI:**
- `OPENAI_API_KEY` (official SDK uses this)

**Stripe:**
- `STRIPE_SECRET_KEY` and `STRIPE_PUBLISHABLE_KEY` (two different keys)

**When in doubt**, check the official SDK documentation for the service's recommended variable names.

## Loading Examples

### Python with dotenv

```python
import os
from dotenv import load_dotenv

# Load from .env file
load_dotenv()

# Access keys
notion_key = os.environ.get("NOTION_API_KEY")
typefully_key = os.environ.get("TYPEFULLY_API_KEY_PERSONAL")
```

### JavaScript/Node.js with dotenv

```javascript
require('dotenv').config();

// Access keys
const notionKey = process.env.NOTION_API_KEY;
const typefullyKey = process.env.TYPEFULLY_API_KEY_PERSONAL;
```

### Shell Scripts

```bash
# Load .env file
set -a
source .env
set +a

# Use in script
curl -H "Authorization: Bearer $NOTION_API_KEY" https://api.notion.com/v1/users
```

## Template File Example

Create `.env.template` for sharing configuration structure (without actual keys):

```bash
# API Keys and Secrets
# Copy this to .env and fill in your actual keys

# Notion
NOTION_API_KEY=your_notion_key_here

# Typefully
TYPEFULLY_API_KEY_PERSONAL=your_personal_key_here
TYPEFULLY_API_KEY_BUSINESS=your_business_key_here

# GitHub
GITHUB_TOKEN=your_github_token_here
GITHUB_USERNAME=your_username

# OpenAI
OPENAI_API_KEY=your_openai_key_here
```

**Commit `.env.template` to git**, but never commit `.env` itself.
