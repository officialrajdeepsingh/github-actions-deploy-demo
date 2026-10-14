# GitHub Actions & GitHub CLI (`gh`) Demo

This repository is the companion demo for the freeCodeCamp article:  
👉 **[How to Use the GitHub CLI with GitHub Actions](https://preview.freecodecamp.org/6a47f33cd74594a1f2e8160f)**

It demonstrates how to automate GitHub tasks using GitHub's built-in **GitHub CLI (`gh`)** without relying on third-party marketplace actions.

---

## What This Demo Does

1. **Auto Comment on New Issues** ([`issue-comment.yml`](.github/workflows/issue-comment.yml)):  
   Whenever a new issue is opened, GitHub Actions automatically uses `gh issue comment` to post a welcoming reply to the author.

2. **Deploy & Release Automation** ([`deploy.yml`](.github/workflows/deploy.yml)):
   - **On Success:** Creates a release with build assets using `gh release create` and opens an announcement issue using `gh issue create`.
   - **On Failure:** Opens an issue reporting deployment failure with direct run logs using `gh issue create`.

---

## How to Test with GitHub CLI

### 1. Test Auto-Comment on Issues

Create a new issue directly from your terminal:

```bash
gh issue create --title "Hello from terminal" --body "Testing the auto-comment action."
```

GitHub Actions will trigger automatically and post a comment. Check it with:

```bash
gh issue view 1 --comments
```

### 2. Test Deploy & Release Workflow

Run the workflow manually from the CLI:

```bash
# Run success path (creates a Release + Success Issue)
gh workflow run deploy.yml

# Run failure path (simulates failure + creates Failure Issue)
gh workflow run deploy.yml -f simulate_failure=true
```

View the generated release:

```bash
gh release list
```
