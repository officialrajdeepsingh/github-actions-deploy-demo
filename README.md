# GitHub Actions & GitHub CLI (`gh`) Demo

This repository is the companion demo for the freeCodeCamp article:  
👉 **[How to Use the GitHub CLI with GitHub Actions](https://preview.freecodecamp.org/6a47f33cd74594a1f2e8160f)**

It demonstrates how to replace third-party GitHub Marketplace actions with GitHub's native **GitHub CLI (`gh`)** for releases, issues, and automated comments.

---

## Why Use GitHub CLI in Workflows?

As covered in the article, many developers reach for third-party marketplace actions for simple tasks like opening an issue or creating a release. Using the built-in `gh` CLI instead gives you:

1. **Better Security & Zero Supply Chain Risk**: Built and maintained directly by GitHub—no risk from abandoned or compromised third-party actions.
2. **Pre-Installed on Runners**: `gh` is already installed and ready on all GitHub-hosted runners (`ubuntu-latest`, `windows-latest`, `macos-latest`).
3. **Zero Maintenance**: No need to manage version bumps or review Dependabot PRs for minor action upgrades.
4. **Native Shell Power**: Standard shell commands with full flexibility for flags, dynamic variables, and custom scripts.

---

## Workflows in this Repository

### 1. Build, Deploy & Release Workflow (`.github/workflows/deploy.yml`)
Runs on push to `main` or manual trigger via `workflow_dispatch`:
* **Builds** an asset (`dist/app.txt`) using standard Linux commands (`mkdir`, `echo`, `date`).
* **Deploys** the application (with a `simulate_failure` toggle to easily test both branches).
* **On Success (`if: success()`):**
  * Publishes a new GitHub Release with the build asset attached using `gh release create`.
  * Opens a success notification Issue using `gh issue create`.
* **On Failure (`if: failure()`):**
  * Opens a failure report Issue with direct links to workflow logs using `gh issue create`.

### 2. Auto-Comment on New Issues (`.github/workflows/issue-comment.yml`)
Runs automatically whenever anyone opens a new Issue (`on: issues: types: [opened]`):
* Greets the author by their GitHub username (`@${{ github.event.issue.user.login }}`).
* Posts a welcoming comment on the issue using `gh issue comment`.

---

## Permissions Required

Both workflows use GitHub Actions' built-in `GH_TOKEN: ${{ github.token }}` and declare the minimum necessary permissions:

```yaml
permissions:
  contents: write  # Needed by deploy.yml to publish releases & assets
  issues: write    # Needed by both workflows to create and comment on issues
```

> [!NOTE]
> Make sure your GitHub repository allows Actions to write to the repository:
> Go to **Settings → Actions → General → Workflow permissions** and select **Read and write permissions**.

---

## Quickstart & CLI Commands

### 1. Initialize and Push to GitHub

```bash
git init
git add .
git commit -m "feat: initial commit for actions demo"
git branch -M main

# Create the public GitHub repository and push
gh repo create github-actions-deploy-demo --public --source=. --remote=origin --push
```

### 2. Test the Deploy Workflow

```bash
# Test the Success path (Creates Release + Success Issue)
gh workflow run deploy.yml

# Test the Failure path (Simulates Error + Creates Failure Issue)
gh workflow run deploy.yml -f simulate_failure=true

# Inspect the results
gh release list
gh issue list
```

### 3. Test Auto-Comment on New Issues

Open a new issue from the terminal:

```bash
gh issue create --title "Testing auto comment" --body "Hello from the CLI!"
```

The `issue-comment.yml` workflow will trigger immediately and add an automated welcome comment. Check it with:

```bash
gh issue view 1 --comments
```
