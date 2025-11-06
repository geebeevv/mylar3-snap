# Automated Release Workflow

This repository uses an automated workflow to track upstream Mylar3 releases and safely promote them through testing before releasing to users.

## Overview

```
Upstream Release → Beta Testing → Stable Release
     (auto)           (manual)        (auto)
```

## How It Works

### 1. Automatic Detection (Daily)

A GitHub Actions workflow runs daily at 3 AM UTC to check for new upstream releases:

- Fetches the latest release from [mylar3/mylar3](https://github.com/mylar3/mylar3)
- Compares with the current version in `snapcraft.yaml`
- If a new version is detected, proceeds to update

### 2. Beta Branch Update (Automatic)

When a new version is found:

1. Updates `snapcraft.yaml` on the `beta` branch:
   - Changes `version:` field to new version
   - Updates `source-tag:` to the new release tag
2. Commits and pushes to `beta` branch
3. Creates a Pull Request from `beta` → `main`

### 3. Snapcraft Builds (Automatic)

When the `beta` branch is updated:

- Snapcraft.io (or Launchpad) automatically builds the snap
- The build is published to the **beta** or **edge** channel
- Users can test with: `snap install geebeevv-mylar3 --beta`

### 4. Pull Request Review (Manual)

The auto-generated PR includes:

- **Upstream release notes** - What changed in the new version
- **Release metadata** - Version, publish date, pre-release flag
- **Testing checklist** - Items to verify before promoting
- **Direct link** to upstream release

**Your job:**

1. Review the upstream changes for:
   - Breaking changes
   - Security fixes (fast-track these!)
   - New dependencies or requirements
2. Test the beta channel build:
   ```bash
   snap refresh geebeevv-mylar3 --beta
   sudo snap logs geebeevv-mylar3
   # Test at http://localhost:8090
   ```
3. Verify the checklist items in the PR
4. Merge when satisfied

### 5. Stable Release (Automatic)

When you merge the PR:

- The `main` branch is updated
- Snapcraft automatically builds from `main`
- The new version is published to the **stable** channel
- Users on stable get the update

## Branch Strategy

- **`main`** - Stable releases, builds to `stable` channel
- **`beta`** - Testing releases, builds to `beta`/`edge` channel

## Snap Channels

| Channel | Purpose | Install Command |
|---------|---------|----------------|
| `stable` | Production (default) | `snap install geebeevv-mylar3` |
| `beta` | Testing before stable | `snap install geebeevv-mylar3 --beta` |
| `edge` | Bleeding edge/daily | `snap install geebeevv-mylar3 --edge` |

## Manual Trigger

You can manually trigger the upstream check without waiting for the daily schedule:

1. Go to **Actions** tab in GitHub
2. Select **Track Upstream Mylar3 Releases** workflow
3. Click **Run workflow**
4. Select `main` branch
5. Click **Run workflow**

## Release Cadence

Mylar3 releases are relatively infrequent compared to other projects. The automated workflow ensures you don't miss releases even with months between updates.

## Security Releases

If an upstream release contains security fixes:

1. The workflow will still create a PR as normal
2. **Fast-track the merge** - skip the beta testing period
3. The PR description will indicate if it's a pre-release

## Troubleshooting

### Workflow Not Running

- Check **Actions** tab for errors
- Verify GitHub Actions is enabled for the repository
- Ensure the workflow file is on the `main` branch

### PR Not Created

- Check if a PR from `beta` → `main` already exists
- The workflow won't create duplicates
- Check workflow logs in **Actions** tab

### Snapcraft Not Building

- Verify Snapcraft.io/Launchpad is connected to your repository
- Check that branch-to-channel mappings are configured:
  - `main` → `stable`
  - `beta` → `beta` or `edge`

### Version Mismatch

If the snap version gets out of sync:

1. Manually update `snap/snapcraft.yaml`:
   ```yaml
   version: '0.8.3'
   source-tag: v0.8.3
   ```
2. Commit to appropriate branch
3. The workflow will resume normal operation

## Configuration

### Change Schedule

Edit `.github/workflows/track-upstream.yml`:

```yaml
on:
  schedule:
    - cron: '0 3 * * *'  # Currently 3 AM UTC daily
```

Examples:
- `'0 */12 * * *'` - Every 12 hours
- `'0 0 * * 0'` - Weekly on Sunday
- `'0 3 * * 1'` - Monday at 3 AM

Given Mylar3's slower release cadence, you could even use weekly checks:
- `'0 3 * * 1'` - Once a week on Monday

### Notifications

To get notified of new PRs:

1. Go to repository **Settings** → **Notifications**
2. Enable notifications for Pull Requests
3. Or use GitHub's "Watch" feature

## Best Practices

1. **Don't skip testing** - Always test beta builds before promoting
2. **Read release notes** - Check for breaking changes
3. **Test comic library access** - Verify removable-media permissions work
4. **Fast-track security** - Merge security fixes immediately
5. **Keep branches clean** - Don't make manual changes to `beta` branch

## FAQ

**Q: Can I manually update the version?**
A: Yes! Just edit `snapcraft.yaml` on the appropriate branch and commit. The workflow will continue to track future releases.

**Q: What if I want to skip a version?**
A: Close the PR without merging. The workflow will create a new PR for the next version.

**Q: Can I test locally before beta?**
A: Yes! Build locally with `snapcraft` command before pushing.

**Q: Why is the schedule different from seerr-snap?**
A: To avoid all workflows running at the same time. Mylar3 checks at 3 AM, Seerr at 2 AM.

---

**Workflow file:** `.github/workflows/track-upstream.yml`
**Maintainer:** geebeevv
**Upstream:** https://github.com/mylar3/mylar3
