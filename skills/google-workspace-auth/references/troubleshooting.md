# Troubleshooting

## Missing OAuth Redirect URI

**Error:**
```
Missing required parameter: redirect_uri
```

**Solution:**
1. Go to Google Cloud Console
2. Open your OAuth client
3. Add redirect URIs:
   - `http://localhost`
   - `http://localhost:8080`
4. Save and retry authentication

## Invalid or Expired OAuth State

**Error:**
```
Invalid or expired OAuth state parameter
```

**Solution:**
1. Close all browser tabs with Google auth
2. Re-run: `make google-auth ACCOUNT=accountN`
3. Use only the newest browser tab that opens

## Insufficient Permissions

**Error:**
```
insufficientPermissions
```

**Solution:**
1. Check that all required scopes are enabled in OAuth consent screen
2. Re-authenticate to grant new scopes

## Localhost Refused to Connect

**Error:**
```
localhost refused to connect
```

**Solution:**
1. Ensure redirect URI includes port: `http://localhost:8080`
2. Re-run authentication command
3. Check firewall isn't blocking port 8080

## Missing Environment Variables

**Error:**
```
Error: Missing environment variables for accountN
```

**Solution:**
1. Check .env file exists at repository root
2. Verify all 3 variables are defined for the account:
   - `GOOGLE_ACCOUNTN_EMAIL`
   - `GOOGLE_OAUTH_CLIENT_ID_ACCOUNTN`
   - `GOOGLE_OAUTH_CLIENT_SECRET_ACCOUNTN`
3. Ensure NO SPACES around `=` sign
4. Reload shell: `source ~/.zshrc`

## Account Not Found in Configuration

**Error:**
```
Error: Account 'accountN' not found in google_accounts.yaml
```

**Solution:**
1. Check `config/google_accounts.yaml` exists
2. Verify account alias is defined
3. Re-run skill to create configuration

## Python Module Not Found

**Error:**
```
ModuleNotFoundError: No module named 'yaml'
```

**Solution:**
```bash
pip install pyyaml keyring google-auth google-auth-oauthlib python-dotenv
```

## Credentials Already Exist

**Message:**
```
✓ Valid credentials already exist for accountN
```

**This is not an error** - account is already authenticated. To re-authenticate:
1. Delete credentials from keyring (or re-run auth with `prompt='consent'`)
2. Run: `make google-auth ACCOUNT=accountN`
