# Module 16 - PBIP, Fabric Git Integration, Branching, and Deployment

Time: 40-60 minutes. This optional extension connects the Git concepts from the profile-page lab to Power BI and Microsoft Fabric development.

---

## Learning goals

By the end, you should be able to explain:

- why PBIP is more suitable for source control than a single PBIX file
- how reports and semantic models are represented as files
- how Fabric workspaces connect to Git branches
- why each developer may need an isolated workspace or local development environment
- how pull requests, CI checks, and Fabric deployment pipelines work together

---

## Part A - Understand PBIP

Power BI Desktop projects save report and semantic model definitions as folders and text files rather than only as one opaque PBIX file.

Typical structure:

```text
powerbi/
└── CloudEngineerSkills/
    ├── CloudEngineerSkills.pbip
    ├── CloudEngineerSkills.Report/
    │   ├── definition.pbir
    │   └── definition/
    ├── CloudEngineerSkills.SemanticModel/
    │   ├── definition.pbism
    │   └── definition/
    └── .gitignore
```

The exact files depend on the enabled format and product version.

PBIP benefits:

- text-based change tracking
- separate report and semantic model folders
- meaningful Git diffs
- easier code review
- better merge-conflict isolation, especially with PBIR and TMDL
- automation and CI/CD possibilities

Microsoft Learn:

- PBIP overview: <https://learn.microsoft.com/en-us/power-bi/developer/projects/projects-overview>
- PBIP report folder and PBIR: <https://learn.microsoft.com/en-us/power-bi/developer/projects/projects-report>
- External editing: <https://learn.microsoft.com/en-us/power-bi/developer/projects/projects-external-editing>

Important: PBIP and some associated formats or Fabric item integrations may be in preview. Confirm current support and limitations before using them in production.

---

## Part B - Continue the storyline

Create an optional Power BI report named **Cloud Engineer Skills Dashboard** based on the profile-page story.

Suggested small dataset:

| Skill | Category | Proficiency | Projects |
|---|---|---:|---:|
| Git and GitHub | DevOps | 4 | 5 |
| Azure | Cloud | 4 | 6 |
| Terraform | Infrastructure | 3 | 3 |
| Docker | Containers | 3 | 4 |
| Power BI | Analytics | 3 | 2 |

Suggested report elements:

- skill count card
- projects by category bar chart
- proficiency by skill chart
- profile summary page

Save the work as a Power BI Project under:

```text
powerbi/CloudEngineerSkills/
```

Before committing:

```bash
git status
git diff --stat
```

Review the generated `.gitignore`. PBIP commonly excludes machine-local or cached content such as:

```gitignore
**/.pbi/localSettings.json
**/.pbi/cache.abf
```

Commit:

```bash
git switch -c feature/pbip-skills-dashboard
git add powerbi/CloudEngineerSkills
git commit -m "feat: add cloud engineer skills PBIP project"
git push -u origin feature/pbip-skills-dashboard
```

Open a pull request and review the file-level changes.

---

## Part C - Demonstrate PBIP change history

Make one visible report change, such as renaming a page or adding a visual. Save the project.

Inspect:

```bash
git status
git diff --stat
git diff
```

Ask learners to identify:

- which report definition files changed
- whether the semantic model changed
- whether the diff is understandable
- which generated or local files should not be committed

Use the history tools from Module 10:

```bash
git log --oneline -- powerbi/CloudEngineerSkills
git show --stat <commit-id>
git blame <text-file-in-the-project>
```

Do not manually edit project files unless the specific file and operation are supported. Power BI Desktop validates project content, and not every file supports external editing.

---

## Part D - Fabric Git integration mental model

Fabric Git integration connects a workspace to:

- a Git provider
- a repository
- a branch
- a repository folder

The connection is at workspace level. A workspace is connected to one branch at a time.

The basic sync directions are:

```text
Fabric workspace changes --commit--> Git branch
Git branch changes ------update----> Fabric workspace
```

Use Git integration to:

- back up and version supported Fabric items
- collaborate through branches
- review changes in pull requests
- revert to earlier versions

Microsoft Learn:

- Overview: <https://learn.microsoft.com/en-us/fabric/cicd/git-integration/intro-to-git-integration>
- Process and permissions: <https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-integration-process>
- Branch-based development: <https://learn.microsoft.com/en-us/fabric/cicd/git-integration/manage-branches>

Support varies by Fabric item type and can change. Verify the current supported-items list before planning a production workflow.

---

## Part E - Recommended branching strategy

### Small team

```text
feature/* -> pull request -> main -> shared development workspace
```

- developers use Power BI Desktop or other client tools locally
- each feature uses a short-lived branch
- pull requests review PBIP and other Fabric definitions
- `main` remains releasable

### Larger team with Fabric-native development

```text
developer branch -> developer workspace
                 -> pull request
                 -> integration branch/main -> shared development workspace
                 -> deployment pipeline -> test -> production
```

Each developer uses an isolated workspace connected to their own branch. This avoids developers overwriting one another in a shared runtime workspace.

Recommended branch naming:

```text
feature/report-navigation
feature/semantic-model-measures
fix/sales-total
release/2026-09
hotfix/refresh-failure
```

Keep branches short-lived and integrate frequently.

---

## Part F - Example end-to-end workflow

### 1. Start the work

```bash
git switch main
git pull --ff-only
git switch -c feature/skills-proficiency-visual
```

### 2. Develop

- Open the PBIP project in Power BI Desktop.
- Add or update the visual.
- Save in Power BI Desktop.
- Inspect the changed text files.

### 3. Commit

```bash
git status
git diff --stat
git add powerbi/CloudEngineerSkills
git commit -m "feat: add skills proficiency visual"
git push -u origin feature/skills-proficiency-visual
```

### 4. Validate in CI

Possible checks:

- required PBIP folders and definition files exist
- JSON files parse successfully
- no conflict markers exist
- naming conventions are followed
- semantic model or report validation tools pass
- no secrets or environment-specific credentials are committed

### 5. Review and merge

- inspect the pull request
- review changed report and model definitions
- run automated checks
- test in an isolated workspace or Power BI Desktop
- approve and merge according to branch policy

### 6. Sync the development workspace

Update the Fabric workspace connected to the integration branch.

### 7. Promote through environments

Use a Fabric deployment pipeline:

```text
Development -> Test -> Production
```

Validate after each stage:

- item deployment status
- data source or connection binding
- parameters and environment values
- permissions
- refresh behavior
- report rendering

---

## Part G - Git integration vs deployment pipelines

| Capability | Git integration | Fabric deployment pipeline |
|---|---|---|
| Main purpose | Versioning and collaboration | Environment promotion |
| Organizes by | Repository, branch, folder | Development, test, production stages |
| Typical action | Commit or update | Deploy |
| Review point | Pull request | Deployment approval or validation process |
| History | Git commits and PRs | Deployment history and stage state |
| Best for | Source control and CI | Release management and CD |

Use both:

1. Git and pull requests control what becomes the approved source.
2. Deployment pipelines control how approved content moves across environments.

---

## Part H - Automation options

Depending on organizational standards, automation can use:

- GitHub Actions
- Fabric deployment pipelines
- Fabric REST APIs
- Fabric CLI
- Microsoft-backed `fabric-cicd` tooling

PBIP deployment with `fabric-cicd`:

<https://learn.microsoft.com/en-us/power-bi/developer/projects/projects-deploy-fabric-cicd>

For automated deployments:

- use a managed identity, workload identity, or service principal according to policy
- store no credentials in the repository
- parameterize workspace IDs, connections, and environment values
- grant only required permissions
- include a rollback or redeployment plan

---

## Common conflict guidance for PBIP

- Prefer PBIR and TMDL source-control-friendly structures where supported.
- Avoid two developers editing the same report page or semantic model object at the same time.
- Save and commit small logical changes.
- Review the complete resulting file, not only the conflict markers.
- Reopen and validate the project in Power BI Desktop after resolution.
- If the generated definitions are too complex to resolve confidently, recreate the intended change from a known-good project version rather than guessing.

---

## Checkpoint

- You can explain why PBIP improves version control.
- You understand that a Fabric workspace connects to one Git branch at a time.
- You can describe local development and isolated-workspace branching models.
- You can distinguish Git integration from Fabric deployment pipelines.
- You can outline a secure development-to-production workflow.

## Try it / Discuss

1. Which team members should be allowed to connect or switch a production-linked workspace branch?
2. Which PBIP files should trigger specialist review?
3. Where should environment-specific values be stored?
4. Which validations belong in the pull request and which belong after deployment to test?

