# Advanced Git and GitHub Cheat Sheet

## Update and switch branches

```bash
git fetch origin --prune
git switch main
git pull --ff-only
git switch <existing-branch>
git switch -c <new-branch>
git switch --track origin/<remote-branch>
```

## Working changes

```bash
git status
git diff
git diff --staged
git stash push -m "WIP: description"
git stash push -u -m "WIP including untracked files"
git stash list
git stash show --patch stash@{0}
git stash pop
git stash apply stash@{0}
git restore <file>
```

## History and auditing

```bash
git log --graph --oneline --decorate --all
git show <commit>
git show --stat <commit>
git log --follow --oneline -- <file>
git log -p -- <file>
git log -S "exact text" --oneline -- <file>
git log -G "pattern" -p -- <file>
git blame -L <start>,<end> <file>
git reflog
```

## Compare work

```bash
git diff <branch-1>..<branch-2>
git diff main...feature/my-work
git log main..feature/my-work --oneline
git log --left-right --graph --oneline main...feature/my-work
```

## Cherry-pick

```bash
git cherry-pick <commit>
git cherry-pick <commit-1> <commit-2>
git cherry-pick <oldest>^..<newest>
git cherry-pick --continue
git cherry-pick --abort
```

## Merge and conflicts

```bash
git switch main
git merge <feature-branch>
git status
git diff
git grep -n -e '<<<<<<<' -e '=======' -e '>>>>>>>'
git add <resolved-file>
git commit
git merge --abort
```

## Rebase

```bash
git switch <private-feature-branch>
git rebase main
git rebase --continue
git rebase --abort
git rebase -i HEAD~3
git push --force-with-lease
```

## GitHub CLI - pull requests

```bash
gh pr list
gh pr status
gh pr view <number>
gh pr view <number> --web
gh pr diff <number>
gh pr diff <number> --name-only
gh pr checkout <number>
gh pr checks <number>
gh pr review <number> --approve
gh pr review <number> --comment --body "Comment"
gh pr review <number> --request-changes --body "Required change"
gh pr merge <number> --squash --delete-branch
```

## GitHub CLI - Actions

```bash
gh workflow list
gh run list
gh run view
gh run rerun <run-id> --failed
gh pr checks --watch
```

## Recovery guardrails

```bash
# Create a safety pointer before risky work
git branch recovery/before-change

# Recover a previous local state
git reflog
git branch recovery/found <reflog-entry-or-commit>

# Undo a shared commit safely
git revert <commit>
```

## Command choice

| Need | Command |
|---|---|
| Move to a branch | `git switch` |
| Create and move to a branch | `git switch -c` |
| Temporarily shelve work | `git stash` |
| Discard an unstaged file edit | `git restore` |
| Copy one commit | `git cherry-pick` |
| Combine branch history | `git merge` |
| Replay a private branch on a new base | `git rebase` |
| Find a past change | `git log`, `git show`, `git blame` |
| Recover local branch movement | `git reflog` |
| Review a PR locally | `gh pr checkout` |

## Safe workflow

```text
fetch -> branch -> edit -> diff -> commit -> push -> PR
      -> review -> checks -> merge -> delete branch
```

