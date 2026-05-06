# GitHub App Setup for PR Auto-Approval

This repository uses a GitHub App for PR auto-approval and labeling. Follow these steps to set it up.

## Why GitHub App?

- ✅ More granular permissions
- ✅ Higher rate limits
- ✅ Appears as a bot, not a user
- ✅ Doesn't count against user seats
- ✅ No token expiration issues
- ✅ Better audit trail

## Setup Instructions

### 1. Create the GitHub App

1. Go to your organization settings: `https://github.com/organizations/YOUR-ORG/settings/apps`
2. Click **New GitHub App**
3. Fill in the details:
   - **Name**: `PR Auto Approver` (or your preferred name)
   - **Homepage URL**: Your organization URL
   - **Webhook**: Uncheck "Active" (not needed)
4. Set **Repository permissions**:
   - Pull requests: **Read & write**
   - Contents: **Read-only**
   - Metadata: **Read-only** (automatically selected)
5. Under **Where can this GitHub App be installed?**:
   - Select "Only on this account"
6. Click **Create GitHub App**

### 2. Generate and Save Private Key

1. After creation, scroll down to **Private keys**
2. Click **Generate a private key**
3. Save the downloaded `.pem` file securely

### 3. Install the App

1. Click **Install App** in the left sidebar
2. Click **Install** next to your organization
3. Choose repositories:
   - **All repositories** (recommended), OR
   - **Only select repositories** (select specific repos)
4. Click **Install**

### 4. Get Your App Credentials

- **App ID**: Found in the app settings page under "About"
- **Private Key**: The `.pem` file you downloaded in step 2

### 5. Add Secrets to Your Repository

Go to your repository: **Settings → Secrets and variables → Actions → New repository secret**

Add the following secrets:

| Secret Name | Value |
|-------------|-------|
| `AUTO_APPROVE_APP_ID` | Your GitHub App ID (e.g., `123456`) |
| `AUTO_APPROVE_APP_PRIVATE_KEY` | Full contents of the `.pem` file (including `-----BEGIN RSA PRIVATE KEY-----` and `-----END RSA PRIVATE KEY-----`) |

### 6. Optional: Keep PAT as Fallback

You can keep `AUTO_APPROVE_TOKEN` as a fallback. The workflow will:
1. Try to use the GitHub App (if `AUTO_APPROVE_APP_ID` is set)
2. Fall back to `AUTO_APPROVE_TOKEN` if App credentials are not available
3. Fall back to default `GITHUB_TOKEN` (limited permissions)

## Verification

After setup, create a test PR and verify:
- The GitHub App bot appears as the approver (not a user)
- Labels are added correctly to bot PRs
- No permission errors in workflow logs

## Troubleshooting

### "Resource not accessible by integration" error
- Check that the GitHub App has the correct permissions
- Ensure the App is installed on the repository
- Verify the private key is correct (including BEGIN/END lines)

### App not appearing as approver
- Check that `AUTO_APPROVE_APP_ID` and `AUTO_APPROVE_APP_PRIVATE_KEY` secrets are set
- Verify the secrets are accessible to the workflow (check repository settings)

### Rate limit issues
- GitHub Apps have much higher rate limits than PATs
- Check the App's rate limit status: `https://api.github.com/rate_limit` (with App token)
