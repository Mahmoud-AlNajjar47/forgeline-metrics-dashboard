# SCM Workflow Diagram: Branching Strategy and Configuration Item States

## Why This Matters
Junior engineers often ask where their work "lives" at each moment and why they must not edit `main` directly. Software Configuration Management (SCM) answers both questions. A clear picture of how a Configuration Item moves between states, and how feature work stays isolated from the stable baseline, prevents lost work, broken builds and confusing review cycles on the DevOps Metrics Dashboard project.

## Key Terms
- **Version control**: a system that records every change to files over time so any earlier version can be recovered.
- **Software Configuration Management (SCM)**: the discipline of tracking and controlling changes to the things a project is made of.
- **Configuration Item (CI)**: any file or artifact placed under SCM control, such as `config/metrics_config.py` or this README. (Here, "CI" never means a build pipeline.)
- **Repository (repo)**: a project folder whose full change history is stored and tracked by Git.
- **Working copy**: the actual files on your computer that you edit. Changes here are not yet recorded.
- **Staging area**: a holding area where you select exactly which changes go into the next commit.
- **Commit**: a saved snapshot of everything in the staging area, stored in the local repository with a unique Commit ID.
- **Upstream**: the shared repository on GitHub that the whole team treats as the source of truth.
- **Branch**: an independent line of development inside a repository.
- **`main`**: the stable baseline branch; it always holds reviewed, working code.
- **Feature branch**: a short-lived branch created from `main` to isolate one piece of work.

## Core Idea
`main` is the stable baseline. Nobody changes it directly. Every change is made on its own **feature branch**, so unfinished or broken work cannot affect teammates. When the work is finished and verified, it returns to `main` only through a reviewed pull request.

### Diagram 1: Feature branch isolation from `main`

```text
UPSTREAM main (stable baseline)
(A)-----(B)-----(C)-------------------------(M)------> time
                  \                         /
                   \                       /   <-- merged via reviewed pull request
                    (F1)-----(F2)-----(F3)-
LOCAL feature/refresh-interval (isolated work)
```

**Annotations**
- `(A)`, `(B)`, `(C)`: commits already on `main` (the baseline).
- The branch point at `(C)`: `git checkout -b feature/refresh-interval` creates the feature branch from `main`.
- `(F1)` to `(F3)`: commits made only on the feature branch. `main` is untouched while they happen.
- `(M)`: the merge point, created only after a pull request has been approved by a peer.

### Diagram 2: Configuration Item state transitions

```text
+--------------+  git add   +--------------+  git commit  +------------------+  git push  +------------------+
| Working copy | ---------> | Staging area | -----------> | Local repository | ---------> | Upstream (GitHub)|
| (you edit)   |            | (you select) |              | (snapshot saved) |            | (team shared)    |
+--------------+            +--------------+              +------------------+            +------------------+
       ^                                                                                           |
       +------------------------------ git fetch + git merge (bring upstream changes back) --------+
```

**Annotations**
1. **Working copy**: edit the Configuration Item. Git sees the change but has not recorded it.
2. **Staging area**: `git add <file>` marks the change for the next snapshot.
3. **Local repository**: `git commit -m "message"` saves the snapshot with a Commit ID.
4. **Upstream**: `git push` shares the commit on GitHub, where a pull request can be opened.
5. **Return path**: `git fetch` and `git merge` bring teammates' upstream changes into your working copy.

## Outline of Rules Derived from the Diagrams
1. Never commit directly to `main`.
2. Create one feature branch per issue.
3. Move each Configuration Item through all states in order: working copy, staging area, repository, upstream.
4. Merge into `main` only through an approved pull request.
