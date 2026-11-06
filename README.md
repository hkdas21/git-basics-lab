# GH-900 Git Basics Lab - Advanced Continuation Pack

This folder extends the existing `hkdas21/git-basics-lab` repository without changing its original Cloud Engineer Profile Page storyline.

## Where to continue

The existing repository currently ends with:

- `05-Branching-and-Merging.md` - introduces branches and previews merge conflicts
- `06-Pull-Requests.md` - completes a basic pull request workflow
- `07-Undo-and-Rollback.md` - covers safe recovery
- `08-Cheat-Sheet.md` - summarizes the introductory commands

Add the new numbered modules after `08-Cheat-Sheet.md`.

For the most effective live delivery, teach `12-Merge-Conflicts-and-Resolution.md` immediately after Module 05, then return to Module 06. This lets learners experience a real conflict at the moment the existing lab introduces it.

## Suggested learning path

| Module | Topic | Approx. time |
|---|---|---:|
| 09 | Switching branches and working with uncommitted changes | 20 min |
| 10 | Git logs, history investigation, and auditing | 25 min |
| 11 | Cherry-pick and commit recovery | 15 min |
| 12 | Merge conflicts and conflict resolution | 30 min |
| 13 | Merge vs rebase and clean history | 25 min |
| 14 | Advanced pull request review and GitHub CLI | 30 min |
| 15 | GitHub Actions, security, and productivity features | 35 min |
| 16 | PBIP, Fabric Git integration, and deployment workflows | 40-60 min |

## Included files

- `09-Switching-Branches-and-Working-Changes.md`
- `10-Git-Logs-and-Auditing.md`
- `11-Cherry-Pick-a-Specific-Commit.md`
- `12-Merge-Conflicts-and-Resolution.md`
- `13-Merge-vs-Rebase.md`
- `14-Advanced-Pull-Request-Review.md`
- `15-GitHub-Actions-Security-and-Efficiency.md`
- `16-PBIP-Fabric-Git-and-Deployment.md`
- `INSTRUCTOR-DEMO-GUIDE.md`
- `ADVANCED-CHEAT-SHEET.md`
- `README-UPDATE-SNIPPET.md`

## Storyline

Learners keep improving the same Cloud Engineer Profile Page. New work adds:

- a temporary "currently learning" banner
- profile-title changes on competing branches
- a certifications feature moved between branches
- automated validation for the HTML and CSS
- an optional Power BI/Fabric extension called the Cloud Engineer Skills Dashboard

This avoids unrelated sample files and makes every Git operation visible in a project learners already understand.

## Prerequisites

- Completion of the existing Modules 00-08
- Git installed and configured
- A clone of the learner's GitHub repository
- VS Code or another editor
- GitHub CLI for Module 14: <https://cli.github.com/>
- Optional for Module 16: Power BI Desktop with Power BI Project (PBIP) preview features enabled and access to a Fabric workspace

## Important instructor note

The commands assume the default branch is named `main`. If a learner's repository uses a different default branch, substitute that branch name consistently.

