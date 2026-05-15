
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
⚙️ GIT CONFIG
=============

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Set your identity for all repos.

```bash
git config --global core.editor "code --wait"
```

Set VS Code as your default editor.

```bash
git config --global init.defaultBranch main
```

Set default branch name to `main` for new repos.

```bash
git config --list
```

Show all active config values.

```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.lg "log --oneline --graph --decorate --all"
```

Create shortcut aliases.

```bash
git config --global core.autocrlf input   # Linux/macOS
git config --global core.autocrlf true    # Windows
```

Control line-ending conversion.

```bash
git config --local user.email "work@corp.com"
```

Per-repo override (no `--global`).

==================================================
📝 .GITIGNORE
=============

Create a `.gitignore` file in the repo root:

```
# Python
__pycache__/
*.pyc
*.pyo
.env

# Node
node_modules/
dist/

# OS
.DS_Store
Thumbs.db

# IDEs
.vscode/
.idea/
```

```bash
git rm -r --cached .
git add .
git commit -m "fix: apply .gitignore"
```

Force Git to respect a newly added `.gitignore` (remove cached tracked files).

Global ignore file:

```bash
git config --global core.excludesfile ~/.gitignore_global
```

==================================================
🪝 GIT HOOKS
============

Hooks live in `.git/hooks/`. Make them executable with `chmod +x`.

**pre-commit** — run before every commit:

```bash
#!/bin/sh
# .git/hooks/pre-commit
npm run lint || exit 1
```

**commit-msg** — enforce message format:

```bash
#!/bin/sh
# .git/hooks/commit-msg
grep -qE "^(feat|fix|docs|chore|refactor|test|style): .+" "$1" || {
  echo "Commit message must follow Conventional Commits format"
  exit 1
}
```

**pre-push** — block pushes that break tests:

```bash
#!/bin/sh
# .git/hooks/pre-push
npm test || exit 1
```

Use **Husky** for team-shared hooks in Node projects:

```bash
npm install --save-dev husky
npx husky init
```

==================================================
🔐 SSH & HTTPS CREDENTIALS
===========================

Generate SSH key:

```bash
ssh-keygen -t ed25519 -C "you@example.com"
```

Add public key to GitHub → Settings → SSH keys.

Test SSH connection:

```bash
ssh -T git@github.com
```

Switch a repo from HTTPS to SSH:

```bash
git remote set-url origin git@github.com:user/repo.git
```

Store HTTPS credentials (credential helper):

```bash
git config --global credential.helper store      # permanent (plaintext)
git config --global credential.helper cache       # temporary (15 min)
```

==================================================
📊 DIFF & PATCH
===============

```bash
git diff HEAD
```

Diff of all uncommitted changes.

```bash
git diff branch1..branch2
```

Diff between two branches.

```bash
git diff HEAD~3..HEAD -- file.txt
```

Diff a single file across a range of commits.

```bash
git diff --stat
```

Show a summary of changed files and lines.

```bash
git format-patch HEAD~3
```

Create `.patch` files for the last 3 commits.

```bash
git apply fix.patch
git am fix.patch
```

Apply a patch file (apply = no commit metadata, am = with metadata).

==================================================
🗃️ INTERACTIVE STAGING
=======================

```bash
git add -p
```

Interactively stage chunks (hunks) of changes — great for splitting a large edit into focused commits.

Keys in hunk mode:
- `y` — stage this hunk
- `n` — skip this hunk
- `s` — split into smaller hunks
- `e` — manually edit the hunk
- `q` — quit

```bash
git add -i
```

Full interactive staging menu.

==================================================
🚀 WORKTREES
============

Check out multiple branches simultaneously in separate directories:

```bash
git worktree add ../hotfix hotfix-branch
```

```bash
git worktree list
git worktree remove ../hotfix
```

Useful for working on a hotfix without stashing your current work.

==================================================
🧵 CHERRY-PICK ADVANCED
========================

```bash
git cherry-pick abc1234
```

Apply a single commit from any branch.

```bash
git cherry-pick abc1234..def5678
```

Apply a range of commits.

```bash
git cherry-pick --no-commit abc1234
```

Apply the commit's changes without committing (review first).

```bash
git cherry-pick --continue
git cherry-pick --abort
```

Handle conflicts during a cherry-pick.

==================================================
📜 REFLOG & RECOVERY
=====================

```bash
git reflog
```

Full history of HEAD movements — even deleted commits are here for ~90 days.

```bash
git reflog show branch-name
```

Reflog for a specific branch.

```bash
git checkout -b recovered abc1234
```

Restore a deleted branch from a reflog hash.

```bash
git stash list
git stash show stash@{2}
git stash drop stash@{2}
git stash clear
```

Manage stashes.

```bash
git stash push -m "WIP: auth refactor" -- src/auth.py
```

Stash a specific file with a description.

==================================================
🔁 REBASE DEEP DIVE
====================

```bash
git rebase -i HEAD~5
```

Interactive rebase for the last 5 commits. Commands:

| Command | Action |
| ------- | ------ |
| `pick`  | Keep commit as-is |
| `reword`| Edit the commit message |
| `edit`  | Pause to amend the commit |
| `squash`| Combine with previous commit |
| `fixup` | Like squash but discard message |
| `drop`  | Delete the commit entirely |
| `exec`  | Run a shell command |

```bash
git rebase --onto main feature base-feature
```

Rebase a sub-branch onto `main`, excluding `base-feature` commits.

**Golden Rule:** Never rebase commits that have been pushed to a shared branch.

==================================================
🏷️ TAGS — DEEP DIVE
====================

```bash
git tag                          # list all tags
git tag v1.2.3                   # lightweight tag
git tag -a v1.2.3 -m "Release"  # annotated tag (recommended)
git tag -a v1.2.3 abc1234        # tag a specific commit
git show v1.2.3                  # inspect tag
git push origin v1.2.3           # push one tag
git push --tags                  # push all tags
git tag -d v1.2.3                # delete local tag
git push origin --delete v1.2.3  # delete remote tag
```

Semantic versioning guide: `MAJOR.MINOR.PATCH`
- MAJOR — breaking changes
- MINOR — new backward-compatible features
- PATCH — bug fixes

==================================================
📡 FETCH, PULL & PUSH OPTIONS
==============================

```bash
git fetch --all --prune
```

Fetch every remote, delete stale tracking branches.

```bash
git pull --rebase
```

Pull and rebase local commits on top (cleaner than merge).

```bash
git pull --rebase=interactive
```

Interactive pull-rebase.

```bash
git push --force-with-lease
```

Safer than `--force`: fails if someone else pushed in the meantime.

```bash
git push origin HEAD:refs/for/main
```

Push to a Gerrit review branch.

```bash
git push origin :old-branch-name
```

Delete a remote branch (alternative syntax).

==================================================
🔎 SEARCHING THE HISTORY
=========================

```bash
git log --grep="login"
```

Search commit messages.

```bash
git log -S "function handleLogin"
```

Find commits that added/removed a specific string (pickaxe).

```bash
git log --author="Alice"
```

Commits by a specific author.

```bash
git log --since="2 weeks ago" --until="yesterday"
```

Commits within a time range.

```bash
git log --all --full-history -- "**/auth.py"
```

History of a deleted file.

```bash
git grep "TODO" HEAD
```

Search file contents at a specific ref.

==================================================
🧹 CLEANING & MAINTENANCE
==========================

```bash
git gc
```

Run garbage collection (compresses objects, removes loose refs).

```bash
git prune
```

Remove objects not reachable from any ref.

```bash
git fsck
```

Check the integrity of the object database.

```bash
git count-objects -vH
```

Show size of the object store.

```bash
git remote prune origin
```

Delete local stale remote-tracking references.

==================================================
🌐 GITHUB CLI — ADVANCED
=========================

```bash
gh repo fork owner/repo --clone
```

Fork and immediately clone.

```bash
gh pr create --draft --title "WIP: feature" --body "Work in progress"
```

Open a draft PR.

```bash
gh pr review 42 --approve
gh pr review 42 --request-changes --body "Please fix the tests"
```

Approve or request changes on a PR.

```bash
gh pr checks 42
```

View CI status for a PR.

```bash
gh issue edit 10 --add-label "bug" --milestone "v2.0"
```

Edit issue metadata.

```bash
gh gist create file.txt --public
```

Create a public Gist.

```bash
gh repo set-default owner/repo
```

Set the default repo for CLI commands.

```bash
gh auth status
gh auth refresh -s delete_repo
```

Check auth or add OAuth scopes.

==================================================
🔄 MERGE STRATEGIES
====================

```bash
git merge --no-ff feature
```

Force a merge commit even if fast-forward is possible (preserves branch history).

```bash
git merge --squash feature
git commit -m "feat: add payment module"
```

Squash all commits from a branch into one commit on the target branch.

```bash
git merge -X ours feature
git merge -X theirs feature
```

Automatically resolve conflicts by preferring "our" or "their" changes.

```bash
git merge --abort
```

Abort an in-progress conflicted merge.

Conflict markers in a file:

```
<<<<<<< HEAD
your code
=======
their code
>>>>>>> feature
```

After resolving:

```bash
git add resolved-file.txt
git commit
```

==================================================
🛡️ SIGNING COMMITS (GPG)
=========================

```bash
gpg --list-secret-keys --keyid-format LONG
git config --global user.signingkey <KEY_ID>
git config --global commit.gpgsign true
```

Sign all commits automatically.

```bash
git commit -S -m "signed commit"
git log --show-signature
```

Sign and verify.

Add GPG public key to GitHub: Settings → SSH and GPG keys.

==================================================
📦 GIT SUBMODULES — DEEP DIVE
==============================

```bash
git submodule add https://github.com/user/lib.git libs/lib
```

Add a submodule.

```bash
git clone --recurse-submodules <url>
```

Clone with all submodules initialized.

```bash
git submodule update --init --recursive
```

Initialize and update after cloning without `--recurse-submodules`.

```bash
git submodule foreach git pull origin main
```

Update all submodules to latest.

```bash
git submodule deinit libs/lib
git rm libs/lib
rm -rf .git/modules/libs/lib
```

Remove a submodule completely.

==================================================
🌍 GITLAB CLI (glab)
====================

```bash
glab auth login
glab repo clone owner/repo
glab mr create --title "feat: payment"
glab mr list
glab mr merge 7
glab issue create --title "Bug" --label bug
glab pipeline list
glab pipeline run
glab ci view
```

==================================================
📋 CONVENTIONAL COMMITS REFERENCE
===================================

Format: `<type>(<scope>): <subject>`

| Type | Usage |
| ---- | ----- |
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Formatting, no logic change |
| `refactor` | Code change without bug fix or feature |
| `test` | Adding or fixing tests |
| `chore` | Build process, dependencies |
| `perf` | Performance improvement |
| `ci` | CI/CD configuration |
| `revert` | Reverts a previous commit |

Examples:

```
feat(auth): add OAuth2 login
fix(api): handle null response from /users
docs: update README installation steps
chore(deps): bump lodash to 4.17.21
```

Breaking change (adds `!` or a `BREAKING CHANGE` footer):

```
feat!: remove deprecated /v1 API endpoint

BREAKING CHANGE: /v1 endpoints no longer exist. Migrate to /v2.
```

==================================================
🗺️ GIT FLOW REFERENCE
======================

Branches:

| Branch | Purpose |
| ------ | ------- |
| `main` | Production-ready code |
| `develop` | Integration branch |
| `feature/*` | New features |
| `release/*` | Release prep |
| `hotfix/*` | Production bug fixes |

```bash
# Start a feature
git switch develop
git switch -c feature/login

# Finish a feature
git switch develop
git merge --no-ff feature/login
git branch -d feature/login

# Create a release
git switch -c release/1.2.0 develop
# ... bump version, update changelog ...
git switch main
git merge --no-ff release/1.2.0
git tag -a v1.2.0 -m "Release 1.2.0"
git switch develop
git merge --no-ff release/1.2.0
git branch -d release/1.2.0
```

==================================================
💡 TIPS & TRICKS
================

```bash
git log --oneline -10
```

Quick last-10-commits view.

```bash
git commit --amend --no-edit
```

Add staged changes to the last commit without changing the message.

```bash
git commit --amend -m "new message"
```

Edit the last commit message (before push only).

```bash
git switch -
```

Switch back to the previous branch.

```bash
GIT_SSH_COMMAND="ssh -i ~/.ssh/id_work" git push
```

Use a specific SSH key for one command.

```bash
git log --format="%h %an %s" --since="1 week ago"
```

Custom log format for weekly standup.

```bash
git shortlog -sn
```

Commit count by author.

```bash
git blame -L 10,20 file.txt
```

Blame specific line range.

```bash
git notes add -m "reviewed by Alice" abc1234
```

Attach a note to a commit without changing it.

==================================================
🏁 END
======




