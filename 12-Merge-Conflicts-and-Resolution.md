# Module 12 - Merge Conflicts and Conflict Resolution

Time: about 30 minutes. You will deliberately create a merge conflict, inspect it, resolve it, test the result, and complete the merge.

---

## Why this lab matters

A merge conflict is not Git breaking. It is Git refusing to guess when two lines of development made incompatible edits.

The safest conflict workflow is:

1. stop and read the status
2. identify `ours` and `theirs`
3. decide the correct final content
4. remove all conflict markers
5. test the result
6. stage and complete the operation

---

## Scenario

The Cloud Engineer Profile Page contains this line:

```html
<p class="role">Cloud Engineer</p>
```

Two pieces of work begin from the same version:

- `main` changes the role to `Cloud Platform Engineer`
- `feature/senior-title` changes the role to `Senior Cloud Engineer`

When the feature branch is merged, Git cannot decide which title is correct.

---

## Step 1 - Prepare a clean starting point

```bash
git switch main
git pull --ff-only
git status
```

If your current role text differs, choose one existing line in `index.html` and use that same line consistently in the exercise.

Create the feature branch:

```bash
git switch -c feature/senior-title
```

Change:

```html
<p class="role">Cloud Engineer</p>
```

to:

```html
<p class="role">Senior Cloud Engineer</p>
```

Commit:

```bash
git add index.html
git commit -m "feat: show senior role title"
```

---

## Step 2 - Make a competing change on `main`

```bash
git switch main
```

Change the original role line to:

```html
<p class="role">Cloud Platform Engineer</p>
```

Commit:

```bash
git add index.html
git commit -m "feat: emphasize platform engineering"
```

Display the divergence:

```bash
git log --graph --oneline --decorate --all -8
```

---

## Step 3 - Trigger the conflict

While on `main`, merge the feature:

```bash
git merge feature/senior-title
```

Expected result:

```text
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

Now stop. Do not immediately run another merge or reset.

Inspect:

```bash
git status
git diff
```

---

## Step 4 - Read the conflict markers

Open `index.html`. Git marks the competing changes:

```html
<<<<<<< HEAD
<p class="role">Cloud Platform Engineer</p>
=======
<p class="role">Senior Cloud Engineer</p>
>>>>>>> feature/senior-title
```

During this merge:

- `HEAD` or "ours" is the branch you are merging into: `main`
- "theirs" is the branch being merged: `feature/senior-title`
- the final answer does not have to be exactly one side

Resolve the business intent by combining the ideas:

```html
<p class="role">Senior Cloud Platform Engineer</p>
```

Remove every marker:

- `<<<<<<<`
- `=======`
- `>>>>>>>`

Search for remaining markers:

```bash
git grep -n -e '<<<<<<<' -e '=======' -e '>>>>>>>'
```

No output means no markers remain in tracked files.

---

## Step 5 - Test and complete the merge

Review the resolved difference:

```bash
git diff
```

Open the page in a browser and confirm the role displays correctly.

Stage the resolved file:

```bash
git add index.html
git status
```

Complete the merge:

```bash
git commit
```

Git opens a default merge commit message. Save it, or use:

```bash
git commit -m "merge: resolve profile title changes"
```

Inspect the result:

```bash
git log --graph --oneline --decorate --all -10
```

---

## Abort and restart

If you become unsure before completing the merge:

```bash
git merge --abort
```

This returns the working tree to the state before the merge attempt.

Then inspect both sides before trying again:

```bash
git diff main...feature/senior-title
git log --left-right --oneline main...feature/senior-title
```

---

## Resolve using an editor

VS Code and other Git-aware editors normally show choices such as:

- Accept Current Change
- Accept Incoming Change
- Accept Both Changes
- Compare Changes

These buttons edit the file; they do not understand the business requirement. Always read the final file, test it, and confirm that no conflict markers remain.

---

## Conflict types

| Conflict | Example | Resolution idea |
|---|---|---|
| Content conflict | Both branches edit the same line | Choose or combine the correct content |
| Add/add | Both branches create the same path differently | Merge the file contents manually |
| Modify/delete | One branch edits a file the other deletes | Decide whether the file should exist |
| Rename conflict | Branches rename or edit paths differently | Choose the intended final path and content |
| Binary conflict | Image or binary file changed on both sides | Select one version or recreate the asset |

---

## Preventing unnecessary conflicts

- Pull or fetch before starting work.
- Keep branches short-lived.
- Make small, focused commits.
- Avoid mixing formatting changes with functional changes.
- Agree on file ownership for high-conflict files.
- Use PBIP/PBIR and other source-control-friendly formats instead of opaque binary files when supported.
- Communicate before making broad refactors.

---

## Checkpoint

- You deliberately created a real merge conflict.
- You identified current and incoming changes.
- You produced a valid final line rather than blindly selecting one side.
- You tested, staged, and committed the resolution.
- You know how to abort an unfinished merge.

## Try it / Discuss

1. Why is "Accept Both" not always a valid resolution?
2. What information should a reviewer see in the merge commit or pull request?
3. Which team habits reduce conflict frequency without trying to eliminate parallel work?

