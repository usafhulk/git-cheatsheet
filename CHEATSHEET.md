
# 🧠 Ultimate Git + GitHub CLI + CI/CD Cheatsheet (With Explanations)

> A comprehensive, offline reference for Git, GitHub CLI, branching, issues, PRs,
> recovery, CI/CD (GitHub Actions), and advanced history rewriting.
>
> Mental anchor:
> Git = local engine  
> GitHub/GitLab = hosting + APIs  
> Git alone cannot create repos.

==================================================
GitHub == gh | GitLab == glab
==================================================

## 📦 SETUP

### Install Git
https://git-scm.com  
Installs the Git CLI (version control engine).

### Install GitHub CLI
https://cli.github.com  

Windows:
```bash
winget install --id GitHub.cli
````

### Authenticate

```bash
gh auth login
```

Authenticates GitHub CLI using browser or token.

---

## CORE RULE (MEMORIZE THIS)

If you didn’t clone it, you must CREATE it before pushing.

Git cannot create GitHub repositories.
GitHub CLI can.

==================================================
🚀 NEVER-FAIL NEW REPO (GitHub – RECOMMENDED)
=============================================

This is the **correct default workflow**.

```bash
mkdir project
cd project

git init
git branch -M main

# add files
git add .
git commit -m "init"

gh repo create project --private --source=. --remote=origin --push
```

This:

* creates the GitHub repo
* links `origin`
* pushes your commits
* avoids all “Repository not found” errors

==================================================
🗂️ REPO BASICS
===============

```bash
git init
```

Initialize a new local repo.

```bash
git clone <url>
```

Clone an existing remote repo.

```bash
git status
```

Show working tree + staging status.

```bash
git add .
git add file.txt
```

Stage changes.

```bash
git commit -m "message"
```

Create a commit.

```bash
git push
git push -u origin main
```

Upload commits (and set upstream).

```bash
git pull
```

Fetch + merge.

```bash
git fetch
```

Fetch only (no merge).

==================================================
🔗 REMOTES
==========

```bash
git remote -v
```

List remotes.

```bash
git remote add origin <url>
```

Add remote.

```bash
git remote set-url origin <url>
```

Change remote URL.

```bash
git remote remove origin
```

Remove remote.

==================================================
🌿 BRANCHING
============

```bash
git branch
git branch -a
```

List branches.

```bash
git switch branch
git switch -c branch
```

Switch / create branch.

```bash
git branch -M main
```

Rename current branch to main.

```bash
git branch -d branch
git branch -D branch
```

Delete local branch (safe / force).

```bash
git push origin --delete branch
```

Delete remote branch (GitHub/GitLab).

Auto-cleanup (recommended):

```bash
git config --global fetch.prune true
```

==================================================
🔀 MERGE & REBASE
=================

```bash
git merge feature
```

Merge branch.

```bash
git rebase main
```

Reapply commits on top of main.

```bash
git rebase --continue
git rebase --abort
```

Control rebase flow.

==================================================
🧾 HISTORY & LOGS
=================

```bash
git log
```

Exit with `q`.

```bash
git log --oneline --graph --decorate --all
```

Visual history.

```bash
git show <hash>
git log file.txt
```

Inspect commits/files.

==================================================
⏪ UNDO & RECOVERY
=================

```bash
git restore file.txt
git restore --staged file.txt
```

Undo local/staged changes.

```bash
git reset --soft HEAD~1
git reset --hard HEAD~1
```

Undo commits (soft keeps changes, hard deletes).

```bash
git reflog
```

Recover “lost” commits.

==================================================
🏷️ TAGS
========

```bash
git tag v1.0.0
git push --tags
```

```bash
git tag -d v1.0.0
git push origin --delete v1.0.0
```

==================================================
📁 STASH
========

```bash
git stash
git stash list
git stash apply
git stash pop
```

==================================================
🧼 DIFFS & CLEANUP
==================

```bash
git diff
git diff --staged
```

```bash
git clean -fd
```

Deletes untracked files (dangerous).

==================================================
🧠 SUBMODULES
=============

```bash
git submodule add <url>
git submodule update --init --recursive
```

==================================================
🌍 GITHUB CLI (gh)
==================

Repos:

```bash
gh repo create
gh repo clone owner/repo
gh repo view
```

Issues:

```bash
gh issue create
gh issue list
gh issue view 3
gh issue close 3
gh issue develop 3
```

PRs:

```bash
gh pr create
gh pr create --fill
gh pr list
gh pr view 5
gh pr checkout 5
gh pr merge 5
```

==================================================
📦 RELEASES, PROJECTS, SECRETS
==============================

```bash
gh release create v1.0.0
gh project list
gh project create --title "My Project"
gh label list
gh label create bug --color FF0000
gh repo add-collaborator username
gh secret set API_KEY
```

==================================================
⚙️ GITHUB ACTIONS / CI
======================

```bash
gh workflow list
gh workflow run build.yml
gh run list
gh run view
```

### Minimal CI Template

```yaml
name: CI
on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.11"
      - run: pip install -r requirements.txt
      - run: pytest
```

==================================================
🧠 ADVANCED HISTORY EDITING
===========================

```bash
git rebase -i HEAD~5
```

pick = keep
reword = edit message
squash = merge commits
drop = delete commit

```bash
git cherry-pick <hash>
git blame file.txt
```

==================================================
🔍 BISECT (BUG HUNTING)
=======================

```bash
git bisect start
git bisect bad
git bisect good <hash>
git bisect reset
```

==================================================
☢️ NUCLEAR OPTIONS (DO NOT PANIC)
=================================

```bash
git push --force
```

```bash
git fetch origin
git reset --hard origin/main
```

==================================================
🧭 BEST PRACTICES
=================

* Branch from main
* Use PRs
* Never force-push main
* Fetch + prune regularly
* Commit small and often
* Tag releases
* Use CI before merge

==================================================
🧠 MENTAL MODEL
===============

Working Tree
→ Staging
→ Commit
→ Push
→ PR
→ Merge
→ Tag

==================================================
✅ Local merge (if you prefer CLI)
===============

From Git Bash:
```bash
git checkout main
git pull origin main
git merge testing_continued
git push origin main
```

Then optionally delete the branch:
```bash
git branch -d testing_continued
git push origin --delete testing_continued
```
⚠️ If main moved while you were working

Rebase before merging (cleaner history):
```bash
git checkout testing_continued
git fetch origin
git rebase origin/main
git checkout main
git merge testing_continued
git push origin main
```

==================================================
🏁 END
======




