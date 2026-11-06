# Module 13 - Merge vs Rebase

Time: about 25 minutes. You will integrate the same branch in two different ways and compare the resulting history.

---

## Concept

Both merge and rebase can bring changes from one branch into another, but they tell different history stories.

### Merge

```text
main:    A---B------M
              \    /
feature:       C--D
```

- preserves the original branch history
- may create a merge commit
- does not rewrite existing commits
- is safe for shared branches

### Rebase

```text
before:
main:    A---B---E
              \
feature:       C---D

after:
main:    A---B---E
                  \
feature:           C'---D'
```

- replays feature commits on a new base
- creates new commit IDs for the replayed commits
- produces a linear history
- rewrites history and requires care on published branches

---

## Step 1 - Create a branch that falls behind

Start from `main`:

```bash
git switch main
git pull --ff-only
git switch -c feature/profile-footer
```

Add a footer to `index.html`:

```html
<footer>
  <p>Built while learning Git and GitHub.</p>
</footer>
```

Commit:

```bash
git add index.html
git commit -m "feat: add learning footer"
```

Now return to `main` and make a separate nonconflicting change to `README.md`:

```bash
git switch main
```

Add a short project description to `README.md`, then commit:

```bash
git add README.md
git commit -m "docs: describe the profile page project"
```

View the divergence:

```bash
git log --graph --oneline --decorate --all -10
```

---

## Step 2 - Rebase the feature branch

```bash
git switch feature/profile-footer
git rebase main
```

Because the changes affect different files or lines, the rebase should complete without conflict.

Inspect:

```bash
git log --graph --oneline --decorate --all -10
```

The footer commit now appears after the latest `main` commit and has a new commit ID.

Merge it into `main`:

```bash
git switch main
git merge --ff-only feature/profile-footer
```

Because the branch was rebased directly on `main`, Git can fast-forward.

---

## Step 3 - Understand a rebase conflict

If a rebase pauses with a conflict:

```bash
git status
```

Resolve the file, stage it, and continue:

```bash
git add <resolved-file>
git rebase --continue
```

Skip the current commit only when you understand why it is unnecessary:

```bash
git rebase --skip
```

Cancel the entire rebase:

```bash
git rebase --abort
```

During rebase, "ours" and "theirs" can feel reversed compared with a normal merge because Git is replaying commits. Rely on the file content and `git status`, not only the labels in an editor.

---

## Step 4 - Decide which strategy to use

| Situation | Recommended approach |
|---|---|
| Shared branch used by several people | Merge |
| Private local feature branch needs the latest `main` | Rebase is often useful |
| Team wants explicit merge events and branch context | Merge commit |
| Team requires linear history | Rebase or squash merge, according to policy |
| Branch already reviewed and other people based work on it | Avoid rewriting it |
| Pull request has many tiny "fix typo" commits | Interactive rebase locally or squash merge |

---

## Interactive rebase for local cleanup

Before publishing a branch, you can clean recent local commits:

```bash
git rebase -i HEAD~3
```

Common actions:

- `pick` - keep the commit
- `reword` - change the message
- `squash` - combine with the previous commit and edit the message
- `fixup` - combine and discard the later message
- `drop` - remove the commit

Do not rewrite shared history unless your team explicitly agrees.

---

## Pushing after rebase

If the branch was already pushed, the remote and local histories no longer match. When team policy permits rewriting your own feature branch:

```bash
git push --force-with-lease
```

Use `--force-with-lease`, not plain `--force`. It refuses to overwrite remote changes you have not seen.

Never force-push a protected shared branch such as `main`.

---

## GitHub merge options

Repositories can allow one or more pull request merge methods:

- Create a merge commit - preserves the PR branch structure
- Squash and merge - combines the PR into one commit
- Rebase and merge - replays the PR commits onto the base branch

The best choice is a team policy decision. Consistency matters more than personal preference.

---

## Checkpoint

- You can explain how merge and rebase produce different history.
- You rebased a private feature branch onto `main`.
- You understand why rebasing changes commit IDs.
- You know when `--force-with-lease` may be needed and why it must be used carefully.

## Try it / Discuss

1. Why is rebase safer before a branch is shared?
2. Which GitHub merge method would make one clean commit per pull request?
3. When does preserving a merge commit improve auditability?

