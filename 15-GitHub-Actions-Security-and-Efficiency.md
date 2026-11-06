# Module 15 - GitHub Actions, Security, and Efficient GitHub Usage

Time: about 35 minutes. You will add a simple pull request validation workflow and explore GitHub features that support a clean, secure team workflow.

---

## Concept - workflow, job, step, runner

GitHub Actions is an automation platform built into GitHub.

- A workflow is a YAML file in `.github/workflows`.
- An event triggers the workflow, such as a push or pull request.
- A job is a group of steps.
- A runner is the machine that executes the job.
- A step runs a command or reusable action.

For this profile project, the first pipeline will validate that required files and important HTML elements exist.

GitHub Actions documentation:

- <https://docs.github.com/en/actions>
- <https://docs.github.com/en/actions/get-started/quickstart>

---

## Step 1 - Create a workflow branch

```bash
git switch main
git pull --ff-only
git switch -c feature/profile-validation
mkdir -p .github/workflows
```

Create `.github/workflows/validate-profile.yml`:

```yaml
name: Validate profile page

on:
  pull_request:
    branches: [main]
  push:
    branches: [main]

permissions:
  contents: read

jobs:
  validate:
    runs-on: ubuntu-latest

    steps:
      - name: Check out repository
        uses: actions/checkout@v6

      - name: Confirm required files exist
        run: |
          test -f index.html
          test -f style.css

      - name: Confirm important page sections exist
        run: |
          grep -q "<main" index.html
          grep -q "Skills" index.html
          grep -q "Contact" index.html

      - name: Check for unresolved merge markers
        run: |
          if grep -R -n -E '^(<<<<<<<|=======|>>>>>>>)' --exclude-dir=.git .; then
            echo "Unresolved merge conflict markers found."
            exit 1
          fi
```

Commit and push:

```bash
git add .github/workflows/validate-profile.yml
git commit -m "ci: validate profile page on pull requests"
git push -u origin feature/profile-validation
```

Open a pull request:

```bash
gh pr create --fill
```

Watch the checks:

```bash
gh pr checks --watch
```

---

## Step 2 - Deliberately fail the workflow

On the feature branch, temporarily change `Skills` to `Capabilities` without updating the workflow.

Commit and push:

```bash
git add index.html
git commit -m "test: demonstrate a failed validation"
git push
```

Observe the failed check in the pull request.

Inspect workflow runs:

```bash
gh run list
gh run view
```

Fix either the page or the expected validation, then commit and push again.

This demonstrates a quality gate: the pull request can be blocked until the agreed checks pass.

---

## CI vs CD vs Fabric deployment pipelines

| Term | Purpose | Example |
|---|---|---|
| Continuous Integration (CI) | Validate each change | Test HTML, validate PBIP files, run linting |
| Continuous Delivery (CD) | Keep changes ready for release | Package or stage an approved build |
| Continuous Deployment | Automatically release approved changes | Publish a site after merge |
| Fabric deployment pipeline | Promote Fabric items across stages | Move content from development to test to production |

GitHub Actions can orchestrate CI and CD. Fabric deployment pipelines provide stage-aware promotion for supported Fabric items. They can be used together.

---

## Security practices

### Never commit secrets

Do not store:

- passwords
- personal access tokens
- client secrets
- private keys
- connection strings containing credentials

Use:

- GitHub Actions secrets
- environment secrets
- workload identity or OpenID Connect where supported
- least-privilege service principals

If a secret is committed, deleting the line in a later commit is not enough. Revoke or rotate the secret immediately, then clean history according to organizational procedure.

### Use least privilege in workflows

Declare permissions explicitly:

```yaml
permissions:
  contents: read
```

Grant write permissions only to the jobs that require them.

### Treat third-party actions as dependencies

- Prefer trusted publishers.
- Review what an action does.
- Pin important production actions according to organizational policy, ideally to a full commit SHA.
- Keep dependencies updated.
- Be cautious with workflows triggered by untrusted fork contributions.

### Protect important branches

Require reviews and status checks on `main`. Prevent direct force pushes and accidental deletion.

### Sign and verify where required

Organizations may require signed commits or verified tags for sensitive repositories. This improves provenance but does not replace code review or automated checks.

---

## GitHub features that improve daily work

| Feature | Useful for |
|---|---|
| Issues | Track work, bugs, and decisions |
| Projects | Plan and visualize work across issues and PRs |
| Pull request templates | Standardize author information and testing evidence |
| CODEOWNERS | Automatically request reviewers for paths |
| Rulesets / protected branches | Enforce review and check policies |
| Actions | Automate validation, builds, and deployments |
| Dependabot | Update dependencies and report vulnerable versions |
| Secret scanning | Detect supported secret patterns |
| Code scanning | Find security and quality issues |
| Releases and tags | Mark and distribute versions |
| GitHub Pages | Publish static sites from a repository |
| Discussions | Hold longer community or design conversations |
| Codespaces | Use a cloud development environment |

Availability depends on the repository visibility, plan, and organization policy.

---

## Useful GitHub CLI commands

```bash
# Open the repository in a browser
gh browse

# Create and list issues
gh issue create
gh issue list

# Create a PR using commit information
gh pr create --fill

# See your PR status
gh pr status

# Watch checks
gh pr checks --watch

# List and inspect workflow runs
gh run list
gh run view

# Re-run failed jobs
gh run rerun <run-id> --failed

# List workflows
gh workflow list

# Trigger a workflow that supports workflow_dispatch
gh workflow run <workflow-name>
```

---

## Optional - publish the profile page

Because this is a static HTML/CSS project, GitHub Pages can publish it. Configure Pages according to your repository and organization policy, then show learners how a merge can result in a visible site update.

Do not make production publishing the first automation example. Start with validation so learners understand quality gates before deployment.

---

## Checkpoint

- You created a workflow triggered by pull requests and pushes to `main`.
- You watched a successful and failed run.
- You understand the difference between CI, CD, and Fabric deployment pipelines.
- You can describe secure secret handling and least-privilege workflow permissions.
- You explored GitHub features beyond commits and pull requests.

## Try it / Discuss

1. Which validations would be valuable for a PBIP project?
2. Why should the workflow use explicit permissions?
3. Which GitHub features would reduce manual work for your team?

