# gh-multi-pr

A GitHub CLI extension to create pull requests from a feature branch to multiple target branches simultaneously.

## Why?

When working with multiple environment branches (dev, staging, beta, master), deploying a feature often requires creating separate PRs for each. This extension automates that process:

- Creates PRs from your feature branch to all specified targets
- Detects merge conflicts and creates environment-specific branches when needed
- Skips targets that don't exist on remote
- Shows existing PRs instead of failing

## Installation

```bash
gh extension install devtopher/gh-multi-pr
```

## Usage

```bash
gh multi-pr [feature-branch] [options]
```

If no branch is specified, uses the current branch.

### Options

| Flag | Description |
|------|-------------|
| `-h, --help` | Show help message |
| `-v, --version` | Show version number |
| `-t, --targets` | Comma-separated list of target branches (default: `dev,staging,beta,master`) |
| `-d, --dry-run` | Show what would be done without creating PRs |

### Examples

```bash
# Create PRs from current branch to all default targets
gh multi-pr

# Create PRs from a specific feature branch
gh multi-pr feature/new-login

# Create PRs to specific targets only
gh multi-pr feature/hotfix --targets staging,master

# Preview what would happen (no changes made)
gh multi-pr --dry-run

# Combine options
gh multi-pr feature/experiment -t dev,staging -d
```

## How It Works

1. **Clean check**: Ensures your working tree is clean
2. **Creates temp clone**: Works in an isolated environment to avoid affecting your repo
3. **For each target branch**:
   - Checks if the target exists on remote
   - Tests if a clean merge is possible
   - If conflicts exist, creates an environment-specific branch (e.g., `feature/foo-staging`)
   - Creates a PR (or reports if one already exists)

## Requirements

- [GitHub CLI](https://cli.github.com/) (`gh`) installed and authenticated
- Git repository with a remote named `origin`

## License

MIT

