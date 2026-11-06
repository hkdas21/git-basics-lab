# Module 10 - Git Logs, History Investigation, and Auditing

Time: about 25 minutes. You will answer practical questions such as who changed a line, when a feature appeared, what a commit contained, and how a branch moved.

---

## Scenario

A reviewer asks:

- When was the Skills section added?
- Which commit changed the profile role?
- Who last changed a specific line?
- What happened before the current branch reached this state?

Git stores the evidence. Your job is to ask the right history question.

---

## Step 1 - Start with a readable graph

```bash
git fetch --all --prune
git log --oneline --graph --decorate --all
```

What the options mean:

- `--oneline` shows one compact line per commit.
- `--graph` draws the branch and merge structure.
- `--decorate` shows branch and tag names.
- `--all` includes all local and remote-tracking branches.

Useful short version:

```bash
git log --graph --oneline --decorate --all -15
```

The `-15` limits the output to the latest 15 commits.

---

## Step 2 - Inspect one commit

Copy a commit ID from the log:

```bash
git show <commit-id>
```

Useful variations:

```bash
# Show only summary information
git show --stat <commit-id>

# Show names and status of changed files
git show --name-status <commit-id>

# Show the version of one file at that commit
git show <commit-id>:index.html
```

Commit IDs can usually be shortened as long as the abbreviation is unique.

---

## Step 3 - Find the history of one file

```bash
git log -- index.html
```

Show the patch for every commit that changed the file:

```bash
git log -p -- index.html
```

Follow the file across renames:

```bash
git log --follow -- index.html
```

Show a compact file history:

```bash
git log --oneline --follow -- index.html
```

---

## Step 4 - Find when a specific text value changed

Search for commits that added or removed the exact string `Cloud Engineer`:

```bash
git log -S "Cloud Engineer" --oneline -- index.html
```

`-S` is called a pickaxe search. It finds commits where the number of occurrences of a string changed.

Search changed lines using a regular expression:

```bash
git log -G "Cloud.*Engineer" --oneline -p -- index.html
```

Use:

- `-S` when you know the exact text that was added or removed.
- `-G` when you want to match a pattern in changed lines.

---

## Step 5 - Find who last changed a line

```bash
git blame index.html
```

Limit the output to a line range:

```bash
git blame -L 1,40 index.html
```

`git blame` shows the commit and author associated with each line. It is an investigation tool, not a people-ranking tool. Use it to locate context, then inspect the commit:

```bash
git show <commit-id>
```

---

## Step 6 - Compare branches and commits

Show commits on the feature branch that are not on `main`:

```bash
git log main..feature/learning-banner --oneline
```

Show commits on either side, marking which branch contains them:

```bash
git log --left-right --graph --oneline main...feature/learning-banner
```

Show file differences:

```bash
git diff main...feature/learning-banner
```

The three-dot comparison uses the common ancestor and is usually the most useful view for pull-request-style changes.

---

## Step 7 - Audit branch movement with reflog

```bash
git reflog
```

The reflog records movements of local references such as:

- branch switches
- resets
- rebases
- commits
- deleted or moved branch tips

Example:

```text
a1b2c3d HEAD@{0}: checkout: moving from main to feature/learning-banner
d4e5f6g HEAD@{1}: commit: fix: clarify contact availability
```

Inspect an earlier state:

```bash
git show HEAD@{1}
```

Create a recovery branch before making risky changes:

```bash
git branch recovery/before-cleanup HEAD@{1}
```

Important: reflog is local. It is not the shared audit history on GitHub and can expire.

---

## Git history vs GitHub audit information

| Evidence | What it answers |
|---|---|
| Git commits | What content changed, by whom, and when |
| Pull request timeline | Reviews, comments, approvals, checks, and merge decisions |
| GitHub Actions logs | What automation ran and whether it passed |
| Branch protection or rulesets | What controls were required before merging |
| Organization audit log | Administrative and security events, when available |
| Local reflog | How your own local references moved |

Do not confuse author date with proof that a change was reviewed or deployed. A complete audit story may require the commit, pull request, workflow run, and deployment record.

---

## Useful log formats

```bash
# Show author, relative date, and subject
git log --pretty=format:"%h | %an | %ar | %s"

# Show commits by one author
git log --author="Name or email" --oneline

# Show commits in a date range
git log --since="2 weeks ago" --until="today" --oneline

# Show merge commits
git log --merges --oneline

# Show commits that are not merges
git log --no-merges --oneline
```

---

## Checkpoint

- You can read a branch graph.
- You can inspect a commit and a file at a historical revision.
- You can use `-S`, `-G`, and `git blame` to investigate past changes.
- You understand the difference between shared Git history, GitHub activity, and local reflog history.

## Try it / Discuss

1. Find the commit that first introduced `style.css`.
2. Find all commits that changed `index.html`.
3. Explain why `git blame` should lead to a conversation about the commit, not a judgment about the person.

