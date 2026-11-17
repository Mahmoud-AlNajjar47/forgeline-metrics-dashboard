# DevOps Metrics Dashboard

## Why This Matters
This is ForgeLine's internal DevOps Metrics Dashboard project. Because ForgeLine sells DevOps tooling, our own engineering practices must be as disciplined as those we promise customers. This README defines how to contribute so that work stays traceable, reviewed and safe for the shared baseline.

## Key Terms
- **Version control**: a system that records every change to files so earlier versions can be recovered.
- **Repository (repo)**: the project folder and its full tracked history.
- **Configuration Item (CI)**: any file under version control, such as `config/metrics_config.py`.
- **Working copy**: your editable files on your computer.
- **Staging area**: where you select changes for the next commit.
- **Upstream**: the shared GitHub repository, the team's source of truth.
- **Feature branch**: a short-lived branch created from `main` for one issue.
- **Pull request (PR)**: a GitHub request to merge a branch into `main`, reviewed by a teammate.
- **Linter**: a tool that scans code for errors and style problems without running it.

## Core Idea
`main` is the protected baseline. All work is tracked by a GitHub **issue**, built on a **feature branch**, verified locally, and merged only through a **peer-reviewed pull request**.

## Repository Structure
```text
.
├── README.md                              # contribution guidelines (this file)
├── docs/
│   ├── SCM_Workflow_Diagram.md            # branching strategy and CI state transitions
│   ├── Command_Sequences.txt              # exact command sequences
│   └── Debugging_and_Linting_Summary.md   # breakpoints, call stacks, stack traces, linting
└── config/                                # configuration files (e.g. metrics_config.py)
```

## SCM Rules
1. Never commit directly to `main`.
2. Create one feature branch per issue, named `feature/<short-description>`.
3. Move every Configuration Item through: working copy, staging area, local repository, upstream.
4. Review your changes with `git diff` and `git diff --staged` before pushing.
5. Run the linter locally and fix all errors before opening a pull request.
6. Bring in upstream changes with `git fetch origin` and `git merge origin/main`, and resolve conflicts before requesting review.

Exact commands are in [docs/Command_Sequences.txt](docs/Command_Sequences.txt). The diagrams are in [docs/SCM_Workflow_Diagram.md](docs/SCM_Workflow_Diagram.md).

## GitHub Issue Workflow
Every piece of work starts as an issue.
1. Open the repository on GitHub and click the **Issues** tab.
2. Click **New issue**.
3. Write a clear **title** (for example, "Make dashboard refresh interval configurable").
4. In the description, explain the problem, the expected result and how to verify it.
5. Add **labels** from the right-hand panel: `bug`, `enhancement` or `documentation`.
6. Under **Assignees**, assign the issue to yourself (or the engineer who will do the work).
7. Click **Submit new issue**. Note its number, for example `#12`.
8. Create your feature branch for that issue and mention the number in your commits and PR.

## Pull Request Workflow
1. Push your branch: `git push -u origin feature/<short-description>`.
2. On GitHub, click **Compare & pull request**.
3. Set the base branch to `main` and the compare branch to your feature branch.
4. Write a title and description explaining what changed and why. Add `Closes #12` to link the issue.
5. Confirm the PR checklist below.
6. Request a review from at least one teammate and click **Create pull request**.
7. Respond to review comments by committing fixes to the same branch; the PR updates automatically.
8. Once approved, the PR is merged into `main`.

**How this protects upstream:** `main` only changes through a pull request, so every change is visible, discussed and reviewed first. **Mandatory peer review** means no one can merge their own unreviewed work, which catches defects before they reach the baseline.

### Pull Request Checklist
- [ ] Linter run locally with no errors
- [ ] `git diff` reviewed; no unintended changes
- [ ] Upstream `main` merged in and any conflicts resolved
- [ ] Issue linked in the description
- [ ] At least one reviewer requested

## Debugging and Linting
Use breakpoints and the call stack to find root causes instead of guessing. See [docs/Debugging_and_Linting_Summary.md](docs/Debugging_and_Linting_Summary.md).
