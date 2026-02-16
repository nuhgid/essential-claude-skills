---
name: google-workspace-credential
description: Guide users through creating OAuth credentials in Google Cloud Console for Google Workspace API access. Use when the user needs to set up Google Cloud credentials for the first time, mentions creating OAuth client, or asks how to get Google API credentials before authenticating accounts.
user-invokable: true
disable-model-invocation: false
---

# Google Workspace Credential Setup

Guides users step-by-step through creating OAuth credentials in Google Cloud Console.

## Overview

This skill walks you through generating the Client ID and Client Secret needed for Google Workspace API integration with Claude Code.

**What you'll create:**
- Google Cloud project (free, no billing required)
- OAuth consent screen
- Desktop app credentials

**What you'll get:**
- Client ID: `XXX.apps.googleusercontent.com`
- Client Secret: `GOCSPX-XXXX`

**Next step after this:** `/google-workspace-auth` to authenticate accounts

## Prerequisites

A Google account. The APIs are free with reasonable rate limits - no billing or free trial required.

## Workflow

### Step 1: Create Google Cloud Account

**Navigate to:** https://console.cloud.google.com/

If this is your first time, you'll be asked to create your account.

**IMPORTANT:** You do NOT need to start a free trial or add a credit card. The APIs work fine without billing setup. Decline if prompted.

Instruct user:
```
1. Go to Google Cloud Console: https://console.cloud.google.com/
2. Create account if prompted
3. DECLINE any prompts for free trial or billing
4. Wait for dashboard to load

Type "ready" when you see the dashboard.
```

### Step 2: Create New Project

Instruct user:
```
1. Click "Select a project" at the top
2. Click "New Project"
3. Project name: (anything you want, e.g., "Google Workspace API")
4. Organization: Leave blank if not using organization
5. Click "Create"
6. Wait for project creation
7. Select your new project from the dropdown

Type "ready" when project is selected.
```

### Step 3: Add APIs to Your Project

API calls only work if you've added those specific APIs to your project.

**Easier method (recommended):**

Instruct user:
```
1. Go to API Library: https://console.cloud.google.com/apis/library
2. Click "Enable APIs and services"
3. Scroll down to the "Google Workspace" section
4. You'll see all Google Workspace APIs listed together

Enable these APIs (click each one, then click "Enable"):
- Google Drive API
- Google Calendar API
- Gmail API
- Google Sheets API
- Google Slides API
- Google Tasks API
- Google Docs API
- Google Forms API

Type "ready" when all APIs are enabled.
```

**Alternative method:** Search for each API individually in the search bar.

### Step 4: Configure OAuth Consent Screen

Instruct user:
```
1. Go to: https://console.cloud.google.com/apis/credentials/consent
2. Select audience:
   - External (for personal Gmail accounts)
   - Internal (for Workspace/organization accounts, if available)
3. Click "Create"

4. Fill in required fields:
   - App name: (anything, e.g., "My Google Workspace Integration")
   - User support email: [your email]
   - Developer contact information: [your email again]

5. Click "Save and Continue" through the next screens
   - Scopes: Click "Save and Continue" (default scopes are fine)
   - Summary: Click "Back to Dashboard"

Type "ready" when consent screen is configured.
```

**Note:** You don't need to manually add specific scopes. The default configuration works for most use cases.

### Step 5: Add Test Users

**If you selected "External" audience:**

Instruct user:
```
1. In OAuth consent screen settings
2. Scroll to "Test users" section
3. Click "Add Users"
4. Add your Google Account email address
5. Click "Add"
6. Click "Save"

Type "ready" when test user is added.
```

**If you selected "Internal":** Skip this step.

### Step 6: Create Credentials

Instruct user:
```
1. Go to Credentials: https://console.cloud.google.com/apis/credentials
2. Click "Create Credentials" at the top
3. Select "OAuth client ID"
4. Application type: Select "Desktop app"
5. Name: (anything, e.g., "Desktop Client")
6. Click "Create"

A dialog will appear showing your credentials:
- Client ID: XXX.apps.googleusercontent.com
- Client Secret: GOCSPX-XXXX

DO NOT CLOSE THIS DIALOG YET.

Type "ready" when you see the credentials dialog.
```

### Step 7: Save Your Credentials

**IMPORTANT: Never paste credentials in this chat.**

Instruct user:
```
You now have two pieces of information:

1. Client ID
   Format: 123456789-abc123def456.apps.googleusercontent.com

2. Client Secret
   Format: GOCSPX-RandomStringHere123456

Options for saving:
- Click "Download JSON" to save credentials as a file
- Copy both values to a secure note
- Keep the browser tab open

The JSON file contains both Client ID and Client Secret.

Type "done" when credentials are saved.
```

### Step 8: Next Steps

Instruct user:
```
✅ Credentials created successfully!

You now have:
- Client ID: XXX.apps.googleusercontent.com
- Client Secret: GOCSPX-XXXX

Next step: Run /google-workspace-auth to authenticate your Google accounts

When ready, run: /google-workspace-auth
```

## Key Points

**No billing required:** The APIs work fine without adding payment information. Reasonable free tier rate limits apply.

**Desktop app type:** Always select "Desktop app" (not "Web application" or "TV/Limited input"). This is critical for the OAuth flow to work correctly.

**Test users:** If using "External" audience, you MUST add your email as a test user, or authentication will fail.

**Credentials security:** Download the JSON file and store it securely. You'll need the Client ID and Client Secret for authentication.

## Troubleshooting

**"OAuth consent screen not available":**
- Verify your email address in Google Cloud Console
- Check that you've created a project first

**"Cannot enable APIs":**
- Ensure you've selected the correct project (check project dropdown at top)
- Refresh the page and try again

**"Credentials not working during auth":**
- Verify you selected "Desktop app" (not "Web application")
- Check that test users are added if using "External" audience
- Ensure all required APIs are enabled

**"Download JSON not available":**
- You can manually copy Client ID and Client Secret instead
- The JSON file is optional but recommended for backup

## Related Skills

- `/google-workspace-auth` - Next step: Authenticate your accounts with these credentials
- `/api-store` - Store API keys securely in .env files
