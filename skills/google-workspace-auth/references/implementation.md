# Implementation Details

## File Structure

```
repo-root/
├── .env                              # Environment variables (NOT committed)
├── config/
│   └── google_accounts.yaml          # Account aliases
├── scripts/
│   └── google/
│       ├── auth.py                   # OAuth authentication script
│       └── fetch_gmail.py            # Gmail fetching script
└── Makefile                          # Make targets
```

## Environment Variable Naming

Pattern: `GOOGLE_<SUFFIX>_<ACCOUNTN>`

Examples:
- `GOOGLE_ACCOUNT1_EMAIL`
- `GOOGLE_OAUTH_CLIENT_ID_ACCOUNT1`
- `GOOGLE_OAUTH_CLIENT_SECRET_ACCOUNT1`
- `GOOGLE_ACCOUNT2_EMAIL`
- `GOOGLE_OAUTH_CLIENT_ID_ACCOUNT2`
- etc.

## .env File Formatting

**CRITICAL:**
- NO SPACES around the `=` sign
- Email FIRST, then Client ID, then Client Secret
- Add blank line between accounts for readability

**Correct:**
```bash
# Account 1
GOOGLE_ACCOUNT1_EMAIL=user1@gmail.com
GOOGLE_OAUTH_CLIENT_ID_ACCOUNT1=123456789-abcdef.apps.googleusercontent.com
GOOGLE_OAUTH_CLIENT_SECRET_ACCOUNT1=GOCSPX-abcdef123456

# Account 2
GOOGLE_ACCOUNT2_EMAIL=user2@gmail.com
GOOGLE_OAUTH_CLIENT_ID_ACCOUNT2=987654321-xyz.apps.googleusercontent.com
GOOGLE_OAUTH_CLIENT_SECRET_ACCOUNT2=GOCSPX-xyz987654
```

**Incorrect:**
```bash
# ❌ WRONG - has spaces around =
GOOGLE_ACCOUNT1_EMAIL = nuhgideon@gmail.com

# ❌ WRONG - email not first
GOOGLE_OAUTH_CLIENT_ID_ACCOUNT1=123456789-abcdef.apps.googleusercontent.com
GOOGLE_ACCOUNT1_EMAIL=nuhgideon@gmail.com
```

## Account Alias Pattern

Pattern: `account1`, `account2`, `account3`, ...

This naming keeps setup generic and reusable.

## OAuth Scopes

The auth script requests:
- `openid`
- `https://www.googleapis.com/auth/userinfo.email`
- `https://www.googleapis.com/auth/userinfo.profile`
- `https://www.googleapis.com/auth/gmail.readonly`
- `https://www.googleapis.com/auth/gmail.compose`
- `https://www.googleapis.com/auth/gmail.modify`
- `https://www.googleapis.com/auth/gmail.labels`
- `https://www.googleapis.com/auth/drive`
- `https://www.googleapis.com/auth/drive.readonly`
- `https://www.googleapis.com/auth/drive.file`
- `https://www.googleapis.com/auth/documents.readonly`
- `https://www.googleapis.com/auth/documents`
- `https://www.googleapis.com/auth/spreadsheets.readonly`
- `https://www.googleapis.com/auth/spreadsheets`
- `https://www.googleapis.com/auth/calendar`
- `https://www.googleapis.com/auth/calendar.readonly`
- `https://www.googleapis.com/auth/calendar.events`
- `https://www.googleapis.com/auth/forms.body`
- `https://www.googleapis.com/auth/forms.body.readonly`
- `https://www.googleapis.com/auth/forms.responses.readonly`
- `https://www.googleapis.com/auth/presentations`
- `https://www.googleapis.com/auth/presentations.readonly`
- `https://www.googleapis.com/auth/tasks`
- `https://www.googleapis.com/auth/tasks.readonly`

## Security

**CRITICAL:**
- `.env` file must be in `.gitignore`
- Never commit OAuth credentials to git
- Credentials stored in keyring are encrypted by OS
- Each account has separate tokens

**Verify .gitignore includes:**
```
.env
.env.*
!.env.*.template
```

## Makefile Commands

```bash
# Authenticate account
make google-auth ACCOUNT=account1

# Fetch Gmail
make gmail-fetch ACCOUNT=account1 COUNT=10
make gmail-fetch ACCOUNT=account1 QUERY="is:unread"
```

## Dependencies

**Python packages:**
```bash
pip install google-api-python-client google-auth google-auth-oauthlib python-dotenv pyyaml keyring
```
