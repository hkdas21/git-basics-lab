# Module 14 - Advanced Pull Request Review and GitHub CLI

Time: about 30 minutes. You will fetch a specific branch, check out an open pull request locally, inspect its changes, run it, review it, and merge it safely.

---

## Part A - Pull the latest branch or a specific branch

### Update the current branch

```bash
git pull --ff-only
```

### Update `main` explicitly

```bash
git switch main
git pull --ff-only origin main
```

### Download remote information without changing files

```bash
git fetch origin --prune
```

### Check out a remote branch for the first time

```bash
git fetch origin
git switch --track origin/feature/learning-banner
```

Alternative with an explicit local name:

```bash
git switch -c feature/learning-banner origin/feature/learning-banner
```

### Update one existing local feature branch

```bash
git switch feature/learning-banner
git pull --ff-only
```

Use `fetch` when you want to inspect remote changes first. Use `pull` when you are ready to fetch and integrate into the current branch.

---

## Part B - Review an open pull request from the command line

Install GitHub CLI and authenticate before the lab:

```bash
gh auth status
```

List pull requests:

```bash
gh pr list
```

View one pull request:

```bash
gh pr view <pr-number>
```

Open it in the browser:

```bash
gh pr view <pr-number> --web
```

Inspect changed files:

```bash
gh pr diff <pr-number> --name-only
```

Inspect the full diff:

```bash
gh pr diff <pr-number>
```

Check out the pull request locally:

```bash
gh pr checkout <pr-number>
```

Now the PR branch is available in your working tree. Open `index.html`, run the page, and test the change rather than reviewing only the text diff.

Useful official GitHub CLI references:

- <https://cli.github.com/manual/gh_pr>
- <https://cli.github.com/manual/gh_pr_checkout>
- <https://cli.github.com/manual/gh_pr_diff>
- <https://cli.github.com/manual/gh_pr_review>

---

## Pure Git alternative for a GitHub pull request

If GitHub CLI is unavailable:

```bash
git fetch origin pull/<pr-number>/head:review/pr-<pr-number>
git switch review/pr-<pr-number>
```

This uses GitHub's pull request reference. GitHub CLI is easier and also supports pull requests from forks.

---

## Part C - Use a structured review

Review four areas:

### 1. Intent

- Does the PR description explain what changed and why?
- Is the PR focused on one outcome?
- Is an issue linked when appropriate?

### 2. Change quality

- Read the Files changed view.
- Check for unrelated formatting or generated-file noise.
- Confirm names and commit messages are understandable.
- Look for accidental secrets, credentials, or personal data.

### 3. Validation

- Check out and run the branch.
- Confirm the page still loads.
- Test the new feature.
- Review automated checks with:

```bash
gh pr checks <pr-number>
```

### 4. Integration risk

- Is the branch behind `main`?
- Are there merge conflicts?
- Does the change alter shared files?
- Is documentation updated?
- Can the change be rolled back?

---

## Part D - Submit a review

Approve:

```bash
gh pr review <pr-number> --approve
```

Comment without blocking:

```bash
gh pr review <pr-number> --comment --body "The feature works locally. Please clarify the CSS class name."
```

Request changes:

```bash
gh pr review <pr-number> --request-changes --body "Please remove the temporary debug text before merge."
```

Good review comments are:

- specific
- about the change, not the person
- clear about whether the comment is required or optional
- supported by an example or expected behavior

---

## Part E - Update and merge safely

Update the PR branch from the base branch:

```bash
gh pr update-branch <pr-number>
```

Merge after approvals and checks:

```bash
# Merge commit
gh pr merge <pr-number> --merge

# Squash merge
gh pr merge <pr-number> --squash

# Rebase merge
gh pr merge <pr-number> --rebase
```

Add `--delete-branch` when the branch should be removed after merge:

```bash
gh pr merge <pr-number> --squash --delete-branch
```

Follow the repository's merge policy rather than choosing a method ad hoc.

---

## Pull request author checklist

Use this Markdown in a PR description:

```markdown
## What changed

- Added:
- Updated:
- Removed:

## Why

Explain the learner or business outcome.

## How I tested

- [ ] Opened `index.html`
- [ ] Confirmed the page renders
- [ ] Checked the mobile layout
- [ ] Confirmed automated checks pass

## Reviewer focus

Please pay special attention to:

## Screenshots

Add before/after images for visible changes when useful.
```

---

## Branch protection and rulesets

For `main`, consider requiring:

- a pull request before merging
- at least one approval
- passing status checks
- resolved conversations
- no force pushes
- no branch deletion
- signed commits, where organizational policy requires them

GitHub documentation:

- <https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches>
- <https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/managing-a-branch-protection-rule>

---

## Checkpoint

- You can fetch and switch to a specific branch.
- You can check out an open PR locally.
- You can inspect a PR's diff and checks from the command line.
- You can submit an approval, comment, or change request.
- You understand that branch rules turn team practices into enforceable controls.

## Try it / Discuss

1. What can local testing reveal that the GitHub diff cannot?
2. Why should a pull request remain focused?
3. Which checks should be mandatory before merging into `main`?

