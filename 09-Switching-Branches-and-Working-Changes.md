# Module 09 - Switching Branches and Working with Uncommitted Changes

Time: about 20 minutes. You will start a small change, interrupt yourself with urgent work, and learn when to commit, stash, restore, or switch branches.

---

## Scenario

You are adding a temporary "Currently learning GitHub Actions" banner to the Cloud Engineer Profile Page. Before you finish, an urgent request arrives to correct the Contact section on `main`.

This is normal development work: one task is unfinished, but you need to move safely to another branch.

## Concept - what happens when you switch branches?

Git tries to preserve your uncommitted work when you switch branches.

- If your changes do not conflict with the target branch, Git might allow the switch and carry the changes with you.
- If switching would overwrite your work, Git stops and asks you to commit, stash, or discard the changes.
- Git is protecting your files. Do not treat the warning as an error to bypass.

Check your starting point:

```bash
git switch main
git pull --ff-only
git status
```

`git pull --ff-only` updates your local branch only when Git can move it forward without creating a surprise merge commit.

---

## Step 1 - Create a feature branch

```bash
git switch -c feature/learning-banner
```

Add this line near the top of `<main>` in `index.html`:

```html
<p class="learning-banner">Currently learning: GitHub Actions</p>
```

Do not commit yet.

Inspect the working tree:

```bash
git status
git diff
```

`git status` tells you which files changed. `git diff` shows the exact unstaged line changes.

---

## Step 2 - Try to switch to `main`

```bash
git switch main
```

Two outcomes are possible:

1. Git switches and carries the uncommitted edit with you because the target branch can accept it safely.
2. Git refuses because switching would overwrite or conflict with a file.

Either result is useful. The key lesson is that uncommitted changes belong to the working tree, not permanently to a branch.

Return to the feature branch if needed:

```bash
git switch feature/learning-banner
```

---

## Step 3 - Stash the unfinished work

Stash the change with a meaningful message:

```bash
git stash push -m "WIP: learning banner"
```

Confirm the working tree is clean:

```bash
git status
git stash list
```

Expected stash entry:

```text
stash@{0}: On feature/learning-banner: WIP: learning banner
```

Now switch safely:

```bash
git switch main
```

Make a small urgent correction in the Contact section, then commit it:

```bash
git add index.html
git commit -m "fix: clarify contact availability"
git push origin main
```

If direct pushes to `main` are protected, create a short fix branch and open a pull request instead.

---

## Step 4 - Return and restore the stashed work

```bash
git switch feature/learning-banner
git stash pop
```

`git stash pop` applies the latest stash and removes it from the stash list if the apply succeeds.

Safer alternative:

```bash
git stash apply stash@{0}
```

`apply` keeps the stash entry, which is useful when you want an extra recovery point.

Finish and commit the banner:

```bash
git add index.html
git commit -m "feat: add current learning banner"
git push -u origin feature/learning-banner
```

---

## `git stash` vs `git checkout`

These commands solve different problems.

| Command | Purpose | Effect on uncommitted work |
|---|---|---|
| `git stash` | Temporarily shelves changes | Saves changes and cleans the working tree |
| `git switch <branch>` | Moves between branches | Keeps safe changes or blocks an unsafe switch |
| `git restore <file>` | Discards unstaged file changes | Replaces the file with the selected Git version |
| `git checkout <branch>` | Older command for switching branches | Similar to `git switch` in this use |
| `git checkout -- <file>` | Older command for discarding file changes | Similar to `git restore <file>` and can destroy work |

Modern teaching recommendation:

- Teach `git switch` for branch movement.
- Teach `git restore` for file restoration.
- Explain `git checkout` because learners will see it in older documentation and scripts.

---

## Useful stash commands

```bash
# Include untracked files
git stash push -u -m "WIP: banner and new assets"

# Preview a stash
git stash show --patch stash@{0}

# Apply a specific stash
git stash apply stash@{1}

# Delete a stash after confirming it is no longer needed
git stash drop stash@{0}

# Create a new branch directly from a stash
git stash branch feature/recover-banner stash@{0}
```

---

## Decision guide

| Situation | Best action |
|---|---|
| The work is complete and meaningful | Commit it |
| The work is incomplete and you must switch tasks | Stash it |
| You want to keep the edit while changing branches | Try `git switch`; Git will protect you |
| You definitely want to discard an unstaged change | Use `git restore <file>` after reviewing `git diff` |
| You need to preserve a messy experiment for later | Create a temporary branch and commit it |

---

## Checkpoint

- You understand that uncommitted changes live in the working tree.
- You used `git stash push`, `git stash list`, and `git stash pop`.
- You can explain why `git switch` and `git restore` are clearer than the older multipurpose `git checkout`.
- Your learning-banner work is committed on `feature/learning-banner`.

## Try it / Discuss

1. Why might a temporary commit be safer than a stash for work you must not lose?
2. What is the difference between `git stash pop` and `git stash apply`?
3. Why should you run `git diff` before `git restore`?

