# GitHub Actions Workflows

## Auto-Commit to Main Workflow

### Overview

The `auto-commit.yml` workflow automatically commits and pushes all changes in the repository to the `main` branch whenever code is pushed to any branch. This enables automatic synchronization of all changes to the main branch.

**Path-Based Triggering**: The workflow only triggers when changes are made in the `backstage-instance/` folder. Changes outside this folder will not trigger the workflow.

### How It Works

1. **Trigger**: The workflow is triggered on push to any branch (`**` pattern) **only when files in `backstage-instance/` folder are modified**
2. **Path Validation**: Validates that changes exist in the `backstage-instance/` folder (defensive check)
3. **Checkout**: Checks out the repository code with full history
4. **Git Configuration**: Configures git user name and email for commits
5. **Branch Detection**: Identifies the source branch and commit SHA
6. **Main Branch Checkout**: Checks out or creates the main branch
7. **Merge**: Merges changes from the source branch into main
8. **Change Detection**: Checks if there are any uncommitted changes
9. **Commit**: Stages and commits all changes (if any exist)
10. **Push**: Pushes the committed changes to the main branch

### Workflow Features

- ✅ **Path-Based Triggering**: Only triggers when changes are made in `backstage-instance/` folder
- ✅ **Automatic Synchronization**: All changes are automatically synced to main branch
- ✅ **Change Detection**: Only commits when there are actual changes
- ✅ **Branch Agnostic**: Works with any branch name (main, master, feature branches, etc.)
- ✅ **Detailed Commit Messages**: Includes source branch, commit SHA, timestamp, and actor
- ✅ **Error Handling**: Gracefully handles merge conflicts and empty commits
- ✅ **Security**: Uses GitHub Actions token with appropriate permissions

### Commit Message Format

Each auto-commit includes a descriptive message with:
- Source branch name
- Source commit SHA
- Timestamp (UTC)
- GitHub actor who triggered the push

Example:
```
Auto-commit from branch: feature/new-feature

Source commit: abc123def456
Timestamp: 2024-01-15 10:30:45 UTC
Triggered by: username
```

### Permissions

The workflow requires:
- `contents: write` permission to push to the repository
- `GITHUB_TOKEN` (automatically provided by GitHub Actions)

### Configuration

The workflow uses the following default settings:
- **Git User**: `github-actions[bot]`
- **Git Email**: `github-actions[bot]@users.noreply.github.com`
- **Target Branch**: `main` (falls back to `master` if main doesn't exist)
- **Path Filter**: `backstage-instance/**` (triggers only on changes in this folder)

### Path-Based Triggering

The workflow is configured to trigger only when changes are made in the `backstage-instance/` folder:

- **Triggers**: Changes to any file or subdirectory within `backstage-instance/`
  - Examples: `backstage-instance/app-config.yaml`, `backstage-instance/packages/**`, etc.
- **Skips**: Changes outside `backstage-instance/` folder will not trigger the workflow
- **Includes**: All subdirectories like `backstage-instance/packages/`, `backstage-instance/.github/`, etc.

To modify the path filter, edit the `paths` section in the workflow file:

```yaml
on:
  push:
    branches:
      - '**'
    paths:
      - 'backstage-instance/**'  # Modify this path pattern as needed
```

### Branch Protection

If branch protection rules are enabled on the main branch:
- The workflow will respect these rules
- You may need to configure branch protection to allow GitHub Actions to push
- Consider adding `github-actions[bot]` as an exception in branch protection settings

### Troubleshooting

**Workflow doesn't trigger:**
- Ensure the workflow file is in `.github/workflows/` directory
- Check that the file has `.yml` or `.yaml` extension
- Verify the workflow syntax is correct
- **Verify changes are in `backstage-instance/` folder** - the workflow only triggers for changes in this path

**Push fails:**
- Check repository permissions
- Verify branch protection settings
- Ensure `GITHUB_TOKEN` has write access

**No changes detected:**
- This is normal if there are no uncommitted changes
- The workflow will skip commit and push in this case

**Merge conflicts:**
- The workflow will attempt to merge but may fail on conflicts
- Manual intervention may be required for complex conflicts

### Security Considerations

- Uses GitHub-provided `GITHUB_TOKEN` (no external credentials)
- All actions are logged in GitHub Actions
- Respects repository and branch protection rules
- Commit author is clearly identified as GitHub Actions bot

### Disabling the Workflow

To disable auto-commit:
1. Delete or rename the `auto-commit.yml` file
2. Or add a condition to skip the workflow based on branch name or other criteria

Example to skip on specific branches:
```yaml
- name: Skip if on main branch
  if: github.ref_name == 'main'
  run: exit 1
```
