---
name: google-workspace-auth
description: Authenticate Google Workspace accounts using OAuth and store credentials securely in keyring. Use when the user needs to set up Gmail, Calendar, Drive, or Forms API access, mentions Google authentication, OAuth setup for Google services, or wants to configure multiple Google accounts for API access.
user-invokable: true
disable-model-invocation: false
---

# Google Workspace Authentication

Authenticates Google Workspace accounts using OAuth and stores credentials securely.

## Prerequisites

OAuth Client ID and Client Secret from Google Cloud Console. If you don't have these, run `/google-workspace-credential` first.

## Workflow

Authentication involves these steps:

1. Detect existing accounts
2. Ask how many accounts to add
3. Collect account emails
4. Create .env templates with placeholders
5. Update config/google_accounts.yaml
6. Authenticate each account via terminal
7. Verify success

### Step 1: Detect Existing Accounts

Read `config/google_accounts.yaml` if it exists. Count configured accounts (account1, account2, etc.) and inform user.

### Step 2: Ask How Many Accounts to Add

**If no existing accounts:**
Ask: "How many Google accounts do you want to authenticate?"
Options: 1, 2, 3, More (ask for number)

**If existing accounts found:**
Say: "You have X account(s) configured: account1, account2, ..."
Ask: "Do you want to add another account?"
If yes → Continue with next account number (accountX+1)

### Step 3: Collect Account Emails

For each account to be added:

Ask user:
```
Setting up Account N of M

What is the Google account email you want to authenticate?
(e.g., your.name@gmail.com)

Email address:
```

Wait for user to provide email. Store for next step.

**IMPORTANT: Do NOT ask for Client ID or Client Secret in chat** (security best practice).

### Step 4: Create .env File Template

**Never ask for or expose credentials in chat. Create template with placeholders instead.**

Check if `.env` exists at repository root:
- If not, create it
- If exists, append to it

Add these template lines to `.env` file:
```bash
# Account N (user@gmail.com)
GOOGLE_ACCOUNTN_EMAIL=YOUR_EMAIL_HERE
GOOGLE_OAUTH_CLIENT_ID_ACCOUNTN=YOUR_CLIENT_ID_HERE
GOOGLE_OAUTH_CLIENT_SECRET_ACCOUNTN=YOUR_CLIENT_SECRET_HERE

```

Then instruct user:
```
I've added template lines to your .env file.

Please open .env and replace the placeholder values:
1. GOOGLE_ACCOUNTN_EMAIL=YOUR_EMAIL_HERE → your actual email
2. GOOGLE_OAUTH_CLIENT_ID_ACCOUNTN=YOUR_CLIENT_ID_HERE → your Client ID (ends with .apps.googleusercontent.com)
3. GOOGLE_OAUTH_CLIENT_SECRET_ACCOUNTN=YOUR_CLIENT_SECRET_HERE → your Client Secret (starts with GOCSPX-)

CRITICAL: Do NOT add spaces around the = sign.

When done, type "ready" to continue.
```

Wait for user confirmation before proceeding.

### Step 5: Update config/google_accounts.yaml

Check if `config/google_accounts.yaml` exists:
- If not, create directory and file
- If exists, append to accounts section

Add account alias:
```yaml
  accountN:
    email_env: GOOGLE_ACCOUNTN_EMAIL
    client_id_env: GOOGLE_OAUTH_CLIENT_ID_ACCOUNTN
    client_secret_env: GOOGLE_OAUTH_CLIENT_SECRET_ACCOUNTN
```

### Step 6: Authenticate Each Account

For each account (in order: account1, account2, account3...):

**Instruct user to reload shell:**
"Reload your shell to pick up the new .env variables"

**User must run this command in their terminal (not in chat):**

```
Please open your terminal and run this command:

make google-auth ACCOUNT=accountN

This will:
1. Open a browser window for Google OAuth
2. Ask you to log in with user@gmail.com
3. Request permission for Gmail, Calendar, Drive access
4. Save credentials securely in your keyring

Run the command now, then let me know when it completes.
```

**Expected output (show to user for reference):**
```
🔐 Authenticating account: accountN
✓ Account email: user@gmail.com
🌐 Starting OAuth flow...
   A browser window will open for authentication
   Make sure to log in with: user@gmail.com

✓ Authentication successful!
✓ Credentials saved securely in keyring
✅ Done! Account accountN is now authenticated.
```

Ask: "Did the authentication complete successfully?"
- If no → See [troubleshooting guide](references/troubleshooting.md)
- If yes → Continue to next account or finish

### Step 7: Summary

After all accounts authenticated, show summary:

```
✅ Authentication Complete!

Configured accounts:
  - account1: user1@gmail.com ✓
  - account2: user2@gmail.com ✓
  - account3: user3@gmail.com ✓

Test your authentication:
  make gmail-fetch ACCOUNT=account1 COUNT=10
```

Ask: "Do you want to add another Google account?"
- If yes → Loop back to Step 2
- If no → Done

## Implementation Details

For environment variable naming, file structure, and security notes, see [references/implementation.md](references/implementation.md).

For troubleshooting authentication errors, see [references/troubleshooting.md](references/troubleshooting.md).

## Related Skills

- `/google-workspace-credential` - Create OAuth credentials in Google Cloud Console
