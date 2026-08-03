# GH-900 · Day 1 Lab — Git Basics with a Real Project

> **Lab project:** Build and version-control a **Cloud Engineer Profile Page** (HTML + CSS) from scratch — and learn every core Git concept along the way.
Try this lab to get started with git
---

## Why this lab is different

Most Git tutorials use throwaway files (`cat.txt`, `dog.txt`). You'll forget those by lunch. Instead, **you'll build one real, growing project** — a personal profile page for a cloud engineer — and use Git the way working engineers actually use it: stage changes, commit meaningful steps, push to GitHub, branch for new features, open a pull request, and fix mistakes safely.

By the end, you'll have a **live, version-controlled web page on GitHub** and the muscle memory to use Git on the job.

---

## What you'll learn

| # | Module | Concepts |
|---|--------|----------|
| 01 | Version Control Basics | What is version control · why Git · how Git thinks |
| 02 | Setup & Init | Install/verify Git · configure identity · `git init` |
| 03 | Staging & Commits | Working directory · staging area · local commits · history |
| 04 | GitHub & Push | What is GitHub · remotes · `push` to `origin` |
| 05 | Branching & Merging | Feature branches · `merge` · resolving conflicts |
| 06 | Pull Requests | The PR workflow · review · merge on GitHub |
| 07 | Undo & Rollback | `revert` · `reset` · `amend` · fixing wrong commits |
| 08 | Cheat Sheet | Quick command reference for the road |

There is also an **Instructor Guide** (`09-Instructor-Guide.md`) with timing, talking points, and common student pitfalls.

---

## The running project: `cloud-profile`

You'll create two files and grow them step by step:

```
cloud-profile/
├── index.html     ← the profile page (HTML structure)
└── style.css      ← the styling (added later, on a branch)
```

Every Git concept is tied to a real edit on these files:

| Git concept | What you'll actually do |
|-------------|--------------------------|
| First commit | Create the basic HTML skeleton |
| Staging area | Add a heading and a subtitle, stage selectively |
| Local commit | Save the "About Me" section |
| Push to origin | Publish the page to GitHub |
| Branch | Build a "Skills" section on a feature branch |
| Merge | Bring the Skills section into `main` |
| Pull Request | Add a "Contact" section via a PR + review |
| Rollback / revert | Undo a bad color change safely |
| Undo wrong commit | Fix a commit you made by mistake |

---

## Prerequisites

- A computer where you can install software (Windows, macOS, or Linux).
- A **free GitHub account** — sign up at <https://github.com/join> if you don't have one.
- A text editor — **VS Code** is recommended (<https://code.visualstudio.com>), but Notepad/TextEdit works too.
- A terminal:
  - **Windows:** *Git Bash* (installed with Git) or *PowerShell*
  - **macOS / Linux:** the built-in *Terminal*
- ~90 minutes. No prior Git experience needed.

> 💡 **No coding experience required.** You'll copy/paste the HTML and CSS. The point is the **Git workflow**, not web design.

---

## How to use these files

1. Work through the modules **in order**, `01` → `08`.
2. Each module has:
   - **Concept** — the *why*, in plain language
   - **Steps** — exact commands to type, with expected output
   - **✅ Checkpoint** — confirm you're on track before moving on
   - **🧠 Try it / Discuss** — reinforcement questions
3. Type the commands yourself — don't just read. Git sticks through your fingers, not your eyes.

---

## A note on the two tools you'll use

| Tool | What it is | Where it runs |
|------|------------|---------------|
| **Git** | The version-control *engine* — tracks every change | On **your computer** (local) |
| **GitHub** | A *website* that hosts Git repositories in the cloud | In the **browser** (remote) |

> Git ≠ GitHub. Git is the tool; GitHub is one popular place to store Git projects online (others include GitLab and Azure DevOps). You can use Git with **no** internet at all — GitHub just adds collaboration, backup, and sharing.

---

➡️ **Next:** Open `01-Version-Control-Basics.md`.
