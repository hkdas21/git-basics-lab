# Module 11 - Take a Specific Commit from Another Branch

Time: about 15 minutes. You will move one useful commit from one branch to another without merging every change from the source branch.

---

## Scenario

A branch named `feature/certifications-experiment` contains two commits:

1. a useful Certifications section
2. an unfinished visual experiment that should not be included

You need only the Certifications commit on a clean branch.

The Git operation for this is `cherry-pick`.

---

## Step 1 - Create the source branch and commits

Start from updated `main`:

```bash
git switch main
git pull --ff-only
git switch -c feature/certifications-experiment
```

Add this section inside `<main>` in `index.html`:

```html
<section class="certifications">
  <h2>Certifications</h2>
  <ul>
    <li>GitHub Foundations</li>
    <li>Microsoft Azure Fundamentals</li>
  </ul>
</section>
```

Commit the useful change:

```bash
git add index.html
git commit -m "feat: add certifications section"
```

Now make a second experimental CSS change:

```css
body {
  transform: rotate(1deg);
}
```

Commit it:

```bash
git add style.css
git commit -m "experiment: tilt the entire page"
```

View the branch history:

```bash
git log --oneline -3
```

Copy the commit ID for `feat: add certifications section`.

---

## Step 2 - Create a clean destination branch

```bash
git switch main
git switch -c feature/certifications
```

Confirm that neither source-branch commit is present:

```bash
git log --oneline --all --decorate --graph -8
```

---

## Step 3 - Cherry-pick the useful commit

```bash
git cherry-pick <certifications-commit-id>
```

Git creates a new commit on the current branch with the same change and message. The new commit normally has a different commit ID because it has a different parent.

Verify:

```bash
git log --oneline -3
git show --stat HEAD
```

Confirm that:

- the Certifications section is present
- the page-tilt experiment is not present

Push the clean branch:

```bash
git push -u origin feature/certifications
```

---

## Cherry-pick a range

To copy several consecutive commits:

```bash
git cherry-pick <oldest-commit>^..<newest-commit>
```

To copy multiple nonconsecutive commits:

```bash
git cherry-pick <commit-1> <commit-2> <commit-3>
```

---

## If a cherry-pick conflicts

Git pauses and identifies the conflicted files:

```bash
git status
```

Resolve the files, then continue:

```bash
git add <resolved-file>
git cherry-pick --continue
```

Cancel and return to the pre-cherry-pick state:

```bash
git cherry-pick --abort
```

---

## When to use cherry-pick

Good uses:

- move a small bug fix to a release branch
- recover one commit from an abandoned branch
- copy a targeted change without merging unrelated work

Avoid using it as the default integration strategy. Repeated cherry-picking can duplicate commits and make branch relationships harder to understand. Prefer pull requests and normal merges for complete features.

---

## Checkpoint

- You identified a commit by its ID.
- You copied only that commit to another branch.
- You understand that cherry-pick creates a new commit on the destination branch.
- You know how to continue or abort a conflicted cherry-pick.

## Try it / Discuss

1. Why did the cherry-picked commit receive a different commit ID?
2. When is a merge better than a cherry-pick?
3. How would you recover one good commit from a branch that contains several failed experiments?

