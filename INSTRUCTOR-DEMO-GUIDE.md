# Instructor Demo Guide - Advanced Git, GitHub, PBIP, and Fabric

This guide is for the instructor. Learners should primarily follow the numbered modules.

Every suggested line to say aloud is bold so it is easy to scan while presenting.

---

## 1. Where to continue in the existing course

### Recommended full sequence

Continue after the existing `08-Cheat-Sheet.md`:

1. Module 09 - switching and uncommitted work
2. Module 10 - logs and auditing
3. Module 11 - cherry-pick
4. Module 12 - merge conflicts
5. Module 13 - merge vs rebase
6. Module 14 - advanced PR review and GitHub CLI
7. Module 15 - Actions, security, and GitHub productivity
8. Module 16 - PBIP, Fabric Git integration, and deployment

### Recommended teaching sequence

For a stronger narrative, insert Module 12 directly after the existing Module 05:

```text
05 Branching and Merging
12 Merge Conflicts and Resolution
06 Pull Requests
07 Undo and Rollback
08 Cheat Sheet
09-16 Advanced continuation
```

The existing Module 05 already shows conflict markers as a preview. Creating a real conflict immediately after that preview produces the best learning moment.

---

## 2. Instructor preparation

Before class:

- Clone a clean instructor copy of the repository.
- Confirm `index.html` contains one predictable profile role line.
- Confirm `style.css` exists.
- Install and authenticate GitHub CLI.
- Create one disposable open pull request for the PR checkout demo.
- Confirm GitHub Actions is enabled.
- Decide whether branch protection or rulesets can be demonstrated in the repository.
- If teaching PBIP, prepare Power BI Desktop, a small PBIP project, a Fabric workspace, and any required permissions.
- Keep a backup branch at the end of each major demo.

Create safety branches:

```bash
git branch demo/starting-point
git tag demo-start
```

**"Today we are not learning isolated commands. We are learning how to make decisions when real work becomes messy."**

**"The profile page remains our project. Every advanced command will solve a problem that appears while the project grows."**

---

## 3. Board plan

Keep this lifecycle visible throughout the session:

```text
Issue or task
    |
    v
Short-lived branch
    |
    v
Small commits
    |
    v
Push + pull request
    |
    v
Review + automated checks
    |
    v
Merge into protected main
    |
    v
Deploy or sync to an environment
```

**"Git records versions. GitHub coordinates people and automation around those versions."**

**"A clean workflow is not a workflow with no mistakes. It is a workflow where mistakes are visible, recoverable, and reviewed."**

---

## 4. Demo 1 - switching branches with unfinished work

### Setup

```bash
git switch main
git pull --ff-only
git switch -c feature/learning-banner
```

Edit `index.html` but do not commit.

### Show

```bash
git status
git diff
git switch main
```

If Git allows the switch, emphasize that the working-tree edit traveled with you. If Git blocks it, emphasize that Git protected the file.

**"A branch owns commits. An uncommitted edit is still in my working tree, so I must decide where it belongs."**

**"Git may let this change travel if it is safe, but it will block the switch if the target branch would overwrite my work."**

Return to the feature branch and stash:

```bash
git switch feature/learning-banner
git stash push -m "WIP: learning banner"
git stash list
git switch main
```

**"A stash is a temporary shelf, not a permanent project history."**

**"If this work is valuable or must be shared, a temporary branch and commit is often safer than leaving it only in a stash."**

Restore:

```bash
git switch feature/learning-banner
git stash pop
```

### Explain checkout

Write:

```text
checkout = older multipurpose command
switch   = branch movement
restore  = file restoration
stash    = temporarily shelve work
```

**"`git checkout` is not the opposite of `git stash`. Checkout historically performed several jobs; modern Git separates those jobs into `switch` and `restore`."**

---

## 5. Demo 2 - make history visible

Start with:

```bash
git log --graph --oneline --decorate --all -15
```

Pause on:

- branch labels
- merge commits
- parent-child relationships
- remote-tracking branches

**"This graph is the map. Before I merge, rebase, reset, or recover, I first look at the map."**

Investigate:

```bash
git log --oneline --follow -- index.html
git log -S "Cloud Engineer" --oneline -- index.html
git blame -L 1,40 index.html
git show <commit-id>
```

**"`git blame` gives me a commit to investigate. It is not a tool for blaming a person."**

**"The useful question is not only who touched the line. The useful question is what problem that commit was solving."**

Show reflog:

```bash
git reflog
```

**"The normal log tells the shared commit story. The reflog tells how my local references moved."**

**"Reflog is often the first recovery tool after an accidental reset, rebase, or branch deletion."**

---

## 6. Demo 3 - cherry-pick one useful change

Prepare a source branch with two commits:

- useful Certifications section
- intentionally bad CSS experiment

Show:

```bash
git log --oneline feature/certifications-experiment
git switch main
git switch -c feature/certifications
git cherry-pick <useful-commit-id>
```

**"I do not want the branch. I want one specific change from the branch."**

**"Cherry-pick copies the change by creating a new commit here. It is targeted integration, not a replacement for normal pull requests."**

Compare commit IDs:

```bash
git log --all --oneline --decorate --graph
```

**"The message and patch can match, but the new commit has a different parent, so it receives a different identity."**

---

## 7. Demo 4 - create a merge conflict effectively

This is the central live demo.

### Preparation

Use one short, visible line:

```html
<p class="role">Cloud Engineer</p>
```

Create the feature branch:

```bash
git switch main
git switch -c feature/senior-title
```

Change it to:

```html
<p class="role">Senior Cloud Engineer</p>
```

Commit:

```bash
git add index.html
git commit -m "feat: show senior role title"
```

Change `main`:

```bash
git switch main
```

Use:

```html
<p class="role">Cloud Platform Engineer</p>
```

Commit:

```bash
git add index.html
git commit -m "feat: emphasize platform engineering"
```

### Before merging

```bash
git log --graph --oneline --decorate --all -8
```

**"Both branches began from the same sentence, and both made a valid but incompatible decision."**

**"Git can combine different files and different lines automatically. It asks for help only where it cannot safely infer intent."**

Trigger:

```bash
git merge feature/senior-title
```

After the conflict, stop typing.

**"This pause is important. A conflict is not a signal to keep running commands. It is a signal to inspect the state."**

Run:

```bash
git status
git diff
```

Open the markers and identify each side.

**"`HEAD` is the branch I am currently merging into. The incoming section is the branch I asked Git to merge."**

**"The correct answer is a product decision. It may be current, incoming, both, or a new combination."**

Resolve to:

```html
<p class="role">Senior Cloud Platform Engineer</p>
```

Search for markers:

```bash
git grep -n -e '<<<<<<<' -e '=======' -e '>>>>>>>'
```

Test the page, then:

```bash
git add index.html
git commit -m "merge: resolve profile title changes"
git log --graph --oneline --decorate --all -10
```

**"Conflict resolution is not finished when the markers disappear. It is finished when the result is valid, tested, staged, and committed."**

### Show abort

If time permits, repeat the conflict and demonstrate:

```bash
git merge --abort
```

**"`--abort` is the safe exit when I need to return to the pre-merge state and rethink the resolution."**

---

## 8. Demo 5 - merge vs rebase

Use a nonconflicting feature commit and a new commit on `main`.

Show the graph before rebase:

```bash
git log --graph --oneline --decorate --all -10
```

Then:

```bash
git switch feature/profile-footer
git rebase main
```

Show the graph again.

**"Merge joins histories. Rebase rewrites the feature branch as if it started from the new base."**

**"The file result may be identical, but the history is different."**

**"Rebase is useful for my private feature branch. I avoid rewriting commits that other people may already be using."**

If discussing a published feature branch:

```bash
git push --force-with-lease
```

**"`--force-with-lease` checks that the remote branch still matches what I last observed. It is safer than plain force, but it is still history rewriting."**

---

## 9. Demo 6 - review an open pull request locally

Prepare a real open PR before class.

```bash
gh pr list
gh pr view <number>
gh pr diff <number> --name-only
gh pr diff <number>
gh pr checks <number>
gh pr checkout <number>
```

Run the page.

**"A pull request is not only a web page. It is a branch I can check out, run, test, and inspect locally."**

**"The diff tells me what changed. Running the branch tells me whether the change behaves correctly."**

Submit a review:

```bash
gh pr review <number> --comment --body "Tested locally. Please clarify the CSS class name."
```

If using a teaching PR, demonstrate approval only when appropriate:

```bash
gh pr review <number> --approve
```

### Review language

**"Make the comment about the code and expected behavior, not about the contributor."**

**"State whether a comment is required before merge or an optional suggestion."**

---

## 10. Demo 7 - GitHub Actions as a quality gate

Create the workflow from Module 15 on a feature branch.

Before pushing:

**"Our first pipeline will validate, not deploy. Learners should see the quality gate before they see automated release."**

Push and open the PR:

```bash
gh pr create --fill
gh pr checks --watch
```

Deliberately break one expected string and push.

**"A red check is useful information. It prevents a known problem from silently entering `main`."**

Inspect:

```bash
gh run list
gh run view
```

Fix and push again.

**"The team decides which checks represent the minimum acceptable evidence for a merge."**

### Pipeline explanation

**"CI answers: is this change safe to integrate?"**

**"CD answers: how does an approved change move toward an environment?"**

**"A Fabric deployment pipeline is an environment-promotion mechanism. GitHub Actions can provide validation and orchestration around it."**

---

## 11. Security demonstration

Show the workflow's explicit permission:

```yaml
permissions:
  contents: read
```

**"Automation receives credentials and permissions. Secure pipelines grant only what each job needs."**

Show repository settings for:

- rulesets or branch protection
- required PR reviews
- required checks
- secret scanning, if available
- Dependabot, if relevant

**"Deleting a secret from the latest file does not make an exposed secret safe. Rotate it first."**

**"Never copy a personal token into source code, a screenshot, a demo command, or a workflow file."**

Do not display any real secret during the demonstration.

---

## 12. PBIP and Fabric extension demo

### PBIP

Open the prepared Cloud Engineer Skills Dashboard PBIP project.

Show the folder structure in VS Code.

**"PBIP turns a Power BI project into source-control-friendly report and semantic model definitions."**

**"Instead of reviewing one opaque PBIX file, Git can show which project files changed."**

Make one visible report change, save, then:

```bash
git status
git diff --stat
git diff
```

**"The diff is evidence, but Power BI Desktop remains the validation environment. I reopen and test the project after complex merges."**

### Fabric Git integration

Draw:

```text
Developer branch <-> Developer workspace
                         |
                         v
                    Pull request
                         |
                         v
main/integration branch <-> Shared development workspace
                         |
                         v
             Deployment pipeline
             Dev -> Test -> Production
```

**"A Fabric workspace is a shared runtime and is connected to one branch at a time."**

**"For parallel development, developers need isolation: local client tools, separate workspaces, or a combination."**

**"Git integration handles versioning and collaboration. Deployment pipelines handle promotion across environments."**

### Limitations

**"Fabric and PBIP capabilities evolve. Before production use, verify the current supported item list, preview status, permissions, and cross-workspace limitations."**

---

## 13. Suggested time-boxed delivery plans

### 60-minute advanced Git block

| Time | Activity |
|---:|---|
| 0-10 | Switch, stash, restore |
| 10-20 | Log graph, show, blame, reflog |
| 20-27 | Cherry-pick |
| 27-47 | Merge conflict |
| 47-57 | Merge vs rebase |
| 57-60 | Recap |

### 60-minute GitHub collaboration block

| Time | Activity |
|---:|---|
| 0-10 | Fetch and pull specific branches |
| 10-30 | PR checkout and structured review |
| 30-48 | GitHub Actions quality gate |
| 48-56 | Branch rules and security |
| 56-60 | Recap |

### 60-minute PBIP/Fabric block

| Time | Activity |
|---:|---|
| 0-15 | PBIP structure and Git diff |
| 15-25 | PBIP branch and pull request |
| 25-40 | Fabric Git integration |
| 40-52 | Branching and workspace isolation |
| 52-58 | Deployment pipeline |
| 58-60 | Recap |

---

## 14. Common learner questions

### "Should I always pull before starting?"

**"Fetch or pull before new work so your branch begins from a current base. Use `git pull --ff-only` when you do not want Git to create an unexpected merge commit."**

### "Can I switch branches with changes?"

**"Sometimes, but the changes may travel with your working tree. Commit, stash, or deliberately carry them based on where the work belongs."**

### "Should I use merge or rebase?"

**"Use the team's policy. Merge is safe for shared history. Rebase is useful for cleaning or updating a private feature branch."**

### "Can I resolve a conflict by accepting both?"

**"Only if both pieces form valid final content. The editor button cannot decide the business requirement."**

### "How do I review an open PR locally?"

**"Use `gh pr checkout` with the PR number, then run and test the branch."**

### "How do I know who changed something?"

**"Use file history, pickaxe search, and blame to locate the commit, then inspect the commit and pull request context."**

### "Are Fabric deployment pipelines the same as Git branches?"

**"No. Branches organize source changes. Deployment pipelines promote approved Fabric content across environment stages."**

---

## 15. Final recap script

**"A professional Git workflow is a sequence of small decisions: start from a current base, isolate the work, commit clearly, inspect history, review the change, automate checks, and merge according to policy."**

**"Conflicts are not failures. They are explicit decisions that Git could not safely make for us."**

**"Git gives us recoverable history. GitHub adds collaboration and automation. PBIP and Fabric extend the same principles to analytics artifacts and environments."**

