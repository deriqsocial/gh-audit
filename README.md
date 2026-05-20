# gh-audit

A fast CLI tool to audit your GitHub repositories for signs of unauthorized access — built in response to the [May 2026 GitHub security incident](https://x.com/github/status/...).

## What it checks

| Check | What it looks for |
|-------|-------------------|
| **Collaborators** | Unexpected users with repo access |
| **Invitations** | Pending invitations you didn't send |
| **Deploy Keys** | Especially recently added ones |
| **Webhooks** | Unknown webhook URLs, recently added hooks |
| **Workflow Changes** | Commits touching `.github/workflows/` |
| **Suspicious Commits** | Commits from unlinked accounts, force pushes |
| **Branch Protection** | Missing or weak protection on default branch |
| **Secret Scanning** | Open alerts (if GitHub Advanced Security enabled) |

## Install

```bash
# Download and make executable
curl -fsSL https://raw.githubusercontent.com/anthropics/gh-audit/main/gh-audit -o gh-audit
chmod +x gh-audit
sudo mv gh-audit /usr/local/bin/

# Or just run it directly
bash <(curl -fsSL https://raw.githubusercontent.com/anthropics/gh-audit/main/gh-audit) --all
```

## Requirements

- [GitHub CLI](https://cli.github.com) (`gh`) installed and authenticated
- `jq` for JSON parsing

## Usage

```bash
# Audit current repo (auto-detects from git remote)
gh-audit

# Audit a specific repo
gh-audit myorg/myrepo

# Audit all your repos
gh-audit --all

# Audit an organization
gh-audit --org my-company

# Look back 30 days instead of default 7
gh-audit --all --days 30

# Multiple repos
gh-audit owner/repo1 owner/repo2 owner/repo3
```

## Exit codes

| Code | Meaning |
|------|---------|
| 0 | All clear |
| 1 | Warnings found (review recommended) |
| 2 | Alerts found (action required) |

## What to do if you find something

1. **Rotate your tokens immediately**: `gh auth refresh` and visit https://github.com/settings/tokens
2. **Review and revoke** any unrecognized deploy keys, webhooks, or collaborators
3. **Check your audit log**: https://github.com/settings/security-log
4. **Enable 2FA** if not already: https://github.com/settings/security

## License

MIT
