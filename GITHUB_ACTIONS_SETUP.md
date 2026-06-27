# GitHub Actions Setup Guide

This guide explains how to set up automated Android APK builds for the Forex Pro Journal app using GitHub Actions.

## Step 1: Enable Workflows Permission

1. Go to your repository settings: https://github.com/bitafam/ManusJournalApp/settings
2. Navigate to **Actions** → **General**
3. Under "Workflow permissions", select **"Read and write permissions"**
4. Check **"Allow GitHub Actions to create and approve pull requests"**
5. Click **Save**

## Step 2: Create the Workflow File

1. In your repository, create the directory structure if it doesn't exist:
   ```
   .github/workflows/
   ```

2. Create a new file: `.github/workflows/build-apk.yml`

3. Copy the following content into the file:

```yaml
name: Build Android APK

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main, develop ]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    timeout-minutes: 60

    steps:
    - name: Checkout code
      uses: actions/checkout@v4

    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: '18'
        cache: 'npm'

    - name: Setup Java
      uses: actions/setup-java@v4
      with:
        distribution: 'temurin'
        java-version: '17'

    - name: Setup Android SDK
      uses: android-actions/setup-android@v3

    - name: Install dependencies
      run: npm ci

    - name: Build APK with EAS
      run: |
        npm install -g eas-cli
        eas build --platform android --non-interactive --output=./app.apk
      env:
        EAS_NO_VCS: 1

    - name: Upload APK as artifact
      uses: actions/upload-artifact@v4
      if: success()
      with:
        name: forex-journal-apk
        path: app.apk
        retention-days: 30

    - name: Create Release
      uses: actions/create-release@v1
      if: startsWith(github.ref, 'refs/tags/')
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      with:
        tag_name: ${{ github.ref }}
        release_name: Release ${{ github.ref }}
        draft: false
        prerelease: false

    - name: Upload APK to Release
      uses: actions/upload-release-asset@v1
      if: startsWith(github.ref, 'refs/tags/')
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      with:
        upload_url: ${{ steps.create_release.outputs.upload_url }}
        asset_path: ./app.apk
        asset_name: forex-journal-${{ github.ref_name }}.apk
        asset_content_type: application/vnd.android.package-archive

    - name: Notify on failure
      if: failure()
      run: echo "Build failed! Check the logs above for details."
```

## Step 3: Configure Expo Credentials

1. Create an Expo account at https://expo.dev if you don't have one
2. Generate an Expo token:
   - Go to https://expo.dev/settings/tokens
   - Create a new token
   - Copy the token

3. Add the token to GitHub Secrets:
   - Go to your repository settings: https://github.com/bitafam/ManusJournalApp/settings/secrets/actions
   - Click **New repository secret**
   - Name: `EXPO_TOKEN`
   - Value: (paste your Expo token)
   - Click **Add secret**

## Step 4: Commit and Push

```bash
git add .github/workflows/build-apk.yml
git commit -m "Add GitHub Actions workflow for APK builds"
git push origin main
```

## Step 5: Trigger Your First Build

### Option A: Automatic (on push)
```bash
git push origin main
```

### Option B: Manual (via GitHub UI)
1. Go to your repository
2. Click the **Actions** tab
3. Select **Build Android APK**
4. Click **Run workflow**
5. Select the branch and click **Run workflow**

### Option C: Release (create tag)
```bash
git tag v1.0.0
git push origin v1.0.0
```

## Step 6: Download Your APK

1. Go to the **Actions** tab
2. Click on the latest workflow run
3. Scroll down to **Artifacts**
4. Download **forex-journal-apk**
5. Extract the APK file

## Troubleshooting

### Build Fails with "EAS CLI not found"
- Make sure Node.js is installed
- Run: `npm install -g eas-cli`

### "EXPO_TOKEN not set"
- Check that you've added the token to GitHub Secrets
- Verify the secret name is exactly `EXPO_TOKEN`

### "Android SDK not found"
- The workflow automatically sets up Android SDK
- If it fails, check the logs for more details

### APK Upload Fails
- Ensure you have write permissions in the repository
- Check that the APK file was successfully built

## Environment Variables

You can add additional environment variables by:

1. Going to repository settings → Secrets and variables → Actions
2. Click **New repository secret**
3. Add the variable name and value

Common variables:
- `EXPO_TOKEN`: Your Expo authentication token
- `JAVA_VERSION`: Java version (default: 17)
- `NODE_VERSION`: Node.js version (default: 18)

## Manual Build (Local)

If you want to build locally instead:

```bash
# Install EAS CLI
npm install -g eas-cli

# Login to Expo
eas login

# Build APK
eas build --platform android --non-interactive
```

## Next Steps

- Monitor builds in the **Actions** tab
- Set up branch protection rules to require successful builds
- Configure notifications for build failures
- Share APK with testers via GitHub Releases

## Support

For issues with GitHub Actions, see:
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Expo EAS Build Documentation](https://docs.expo.dev/build/introduction/)
- [Android Build Documentation](https://docs.expo.dev/build-reference/android-build-process/)
