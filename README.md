# GitHub Actions Demo: Releases & Failure Issues

A beginner-friendly demo showing how to use the **GitHub CLI (`gh`)** inside a **GitHub Actions** workflow to:

1. **Build an asset** (`dist/app.txt`).
2. **Deploy an application**.
3. **On Success:** Automatically create a GitHub Release (`v1.0.<run_number>`) and upload the build file using `gh release create`.
4. **On Failure:** Automatically open a GitHub Issue with a direct link to the failed logs using `gh issue create`.

---

## Workflow Setup & Permissions

The workflow declares the minimum permissions needed to create releases and issues:

```yaml
permissions:
  contents: write  # Allows creating releases & tags
  issues: write    # Allows opening issues
```

> [!NOTE]
> Make sure your GitHub repository allows Actions to write to the repository:
> Go to **Settings → Actions → General → Workflow permissions** and select **Read and write permissions**.

---

## Quickstart Commands

### 1. Initialize and Push to GitHub

```bash
git init
git add .
git commit -m "feat: initial commit"
git branch -M main

# Create the public GitHub repository and push
gh repo create github-actions-deploy-demo --public --source=. --remote=origin --push
```

### 2. Test Success Path (Creates Release + Uploads File)

Trigger the workflow:

```bash
gh workflow run deploy.yml
```

Check the newly created release:

```bash
gh release list
```

### 3. Test Failure Path (Simulates Error + Creates Issue)

Trigger the workflow with failure simulation enabled:

```bash
gh workflow run deploy.yml -f simulate_failure=true
```

Check the automatically opened issue:

```bash
gh issue list
```
