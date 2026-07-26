# Git Basics — Complete Reference

> Open this file any time you get stuck. Everything you need is right here.
> Organized from **most important** (top) to **least important** (bottom).

---

# ⚡ PART 1 — USE THIS EVERY DAY

These are the commands you'll use 80% of the time. Master these first.

---

## 1. The Core Workflow

This is the heart of Git. Every time you work, you follow this cycle:

```
Edit files → git add → git commit → git push
```

### Check what changed

```bash
git status
```

**Run this constantly.** Before you add, after you add, after you commit — always check status. It tells you exactly where things stand:

| What you see                           | What it means                                 |
|----------------------------------------|-----------------------------------------------|
| **Red** — "Untracked files"            | New file, Git doesn't know about it yet       |
| **Red** — "Changes not staged for commit" | Existing file was modified but not staged  |
| **Green** — "Changes to be committed"  | File is staged and ready to be committed      |
| "nothing to commit, working tree clean"| Everything is saved. You're up to date.       |

### Stage your changes (add to the waiting room)

Stage one file:

```bash
git add hello_world.txt
```

Stage everything at once:

```bash
git add .
```

The `.` means "everything in the current directory and all subdirectories."

### Commit (save the snapshot)

```bash
git commit -m "Add hello_world.txt"
```

- `-m` = message flag. The text in quotes describes **what you did**.
- Start with a verb: "Add", "Fix", "Update", "Remove", "Refactor".

If you forget `-m`:

```bash
git commit
```

VS Code opens. Write your message, save (`Cmd + S`), close the tab. Done.

### Push to GitHub (upload your work)

```bash
git push
```

Or the full version:

```bash
git push origin main
```

| Part       | Meaning                                |
|------------|----------------------------------------|
| `git push` | The command to upload                  |
| `origin`   | The name of the remote (nickname for GitHub) |
| `main`     | The branch you're pushing              |

If you only have one remote and one branch, `git push` is enough.

### View your commit history

```bash
git log
```

Each entry shows: commit hash, author, date, and message.

If the screen gets stuck and shows `(END)`, press **`q`** to exit.

Shorter version:

```bash
git log --oneline
```

---

## 2. Complete Workflow Example

Here's the entire process from start to finish:

```bash
# 1. Create a file
touch hello_world.txt

# 2. Check status (file is red / untracked)
git status

# 3. Stage the file
git add hello_world.txt

# 4. Check status (file is green / staged)
git status

# 5. Commit
git commit -m "Add hello_world.txt"

# 6. Check status (nothing to commit)
git status

# 7. Edit README.md — add some text, save it

# 8. Edit hello_world.txt — add some text, save it

# 9. Stage everything
git add .

# 10. Commit
git commit -m "Edit README.md and hello_world.txt"

# 11. Push to GitHub
git push

# 12. Done. Confirm.
git status
```

---

## 3. How Git Works — The Two-Stage System

Git does NOT save your work automatically. You go through two steps:

```
Working Directory  →  Staging Area  →  Repository (commit history)
       ↑                   ↑                    ↑
  You edit files      git add           git commit
```

1. **Working Directory** — Your actual files on disk. You edit them here.
2. **Staging Area** — A "waiting room." You pick which changes to include in the next commit.
3. **Commit** — A permanent snapshot saved in Git's history.

**Why two steps?** Because you might change 10 files but only want to commit 3 of them right now. The staging area lets you choose.

---

# 🔧 PART 2 — FIXING MISTAKES & UNDOING THINGS

You will mess up. These commands save you.

---

## 4. Undo Changes (Before Staging)

You edited a file and want to throw away the changes:

```bash
git restore filename.txt
```

Or the older way:

```bash
git checkout -- filename.txt
```

This resets the file back to the last committed version. **Your changes are gone forever.**

## 5. Unstage a File (After `git add`, Before `git commit`)

```bash
git restore --staged filename.txt
```

Or the older way:

```bash
git reset HEAD filename.txt
```

The file goes back to "modified but not staged." Your edits are still there, just removed from the staging area.

## 6. Undo the Last Commit (Keep the Changes)

```bash
git reset --soft HEAD~1
```

The commit is undone, but your files stay modified and staged. You can fix things and commit again.

## 7. Undo the Last Commit (Throw Away Changes)

```bash
git reset --hard HEAD~1
```

**⚠️ WARNING:** This deletes the commit AND your changes. No going back.

## 8. See What Changed

Before staging:

```bash
git diff
```

After staging:

```bash
git diff --staged
```

Shows line-by-line what you changed.

---

# 📁 PART 3 — MANAGING FILES

---

## 9. Rename a File or Folder

```bash
git mv old_name.txt new_name.txt
git commit -m "Rename old_name.txt to new_name.txt"
```

Works for folders too:

```bash
git mv old_folder new_folder
git commit -m "Rename old_folder to new_folder"
```

`git mv` does two things at once: renames the file on disk AND stages the change.

Without Git, you'd have to do this manually:

```bash
mv old_name.txt new_name.txt   # rename on disk
git add new_name.txt            # stage the new name
git rm old_name.txt             # tell Git the old name is gone
```

## 10. Delete a File

```bash
git rm unwanted_file.txt
git commit -m "Remove unwanted_file.txt"
```

This deletes the file from disk AND stages the deletion.

If you want to stop tracking a file but keep it on disk:

```bash
git rm --cached secret_file.txt
```

## 11. Create a File

```bash
touch hello_world.txt
```

Creates an empty file.

## 12. Open Folder in VS Code

```bash
code .
```

Opens the current folder in VS Code for editing.

---

# 🌿 PART 4 — BRANCHES & COLLABORATION

---

## 13. Branches

Branches let you work on features without touching the main code.

```bash
# See all branches
git branch

# Create a new branch
git branch feature-login

# Switch to a branch
git switch feature-login

# Create AND switch in one command
git switch -c feature-login

# Go back to main
git switch main

# Merge a branch into main
git switch main
git merge feature-login

# Delete a branch after merging
git branch -d feature-login
```

Older commands that do the same thing:

```bash
git checkout feature-login          # same as git switch
git checkout -b feature-login       # same as git switch -c
```

## 14. Pull (Download Updates from GitHub)

```bash
git pull
```

Or the full version:

```bash
git pull origin main
```

Downloads any commits from GitHub that you don't have locally. Always pull before you start working if others are contributing to the same repo.

## 15. Stash (Save Work Temporarily)

You're in the middle of something and need to switch branches, but not ready to commit:

```bash
# Save your changes temporarily
git stash

# Do your other work...

# Bring your changes back
git stash pop
```

## 16. See Who Changed What

```bash
git blame filename.txt
```

Shows every line of the file and who last changed it.

---

# 🔗 PART 5 — CLONING & REMOTE SETUP

You do this once per project.

---

## 17. Clone a Repository

### Get the SSH URL from GitHub:

1. Go to the repo page on GitHub.
2. Click the green **"Code"** button.
3. Select the **SSH** tab (not HTTPS).
4. Copy the URL:

```
git@github.com:USER-NAME/REPOSITORY-NAME.git
```

> **⚠️ WARNING:** If your URL starts with `https://`, you picked HTTPS, not SSH. Go back and click SSH.

### Clone it:

```bash
git clone git@github.com:USER-NAME/REPOSITORY-NAME.git
```

### Clone a specific branch:

```bash
git clone -b branch-name git@github.com:USER/REPO.git
```

### Create a project folder first (one time):

```bash
cd ~
mkdir repos
cd repos
```

## 18. Check Your Remote Connection

```bash
git remote -v
```

You should see:

```
origin  git@github.com:USER-NAME/git_test.git (fetch)
origin  git@github.com:USER-NAME/git_test.git (push)
```

- **`origin`** = the default nickname Git gives to the remote you cloned from.
- **`(fetch)`** = URL for downloading changes.
- **`(push)`** = URL for uploading changes.

## 19. Switch from HTTPS to SSH

If you accidentally cloned with HTTPS:

```bash
git remote set-url origin git@github.com:USER-NAME/REPOSITORY-NAME.git
```

---

# ⚙️ PART 6 — FIRST-TIME SETUP

You do these once on your machine. Never again.

---

## 20. Check Git Version

```bash
git --version
```

You need **2.28 or later**.

## 21. Set Your Name and Email

```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

Git attaches this to every commit. Use the same email as your GitHub account.

### Change your name later:

```bash
git config --global user.name "New Name"
```

Verify it:

```bash
git config --global user.name
```

## 22. Set Default Branch to `main`

```bash
git config --global init.defaultBranch main
```

## 23. Set VS Code as Commit Editor

```bash
git config --global core.editor "code --wait"
```

No output after running this — that's normal.

## 24. View All Git Settings

```bash
git config --list
```

---

# 🏗️ PART 7 — CREATING A REPO ON GITHUB

---

## 25. Create a New Repository

1. Go to [github.com](https://github.com).
2. Click the **`+`** button (top-right) → **"New repository"**.
3. Name it (example: `git_test`).
4. Check **"Add a README file"**.
5. Click **"Create repository"**.

Done. You now have a repo on GitHub with a `README.md` file.

---

# 📐 PART 8 — RULES & BEST PRACTICES

---

## 26. Never Edit Files Directly on GitHub

Always edit on your local machine → commit → push. If you edit on GitHub directly, your local copy and GitHub will be out of sync, and fixing that is painful.

## 27. Make Atomic Commits

One commit = one thing.

**Bad:**

```bash
git commit -m "Fix login bug, add footer, update CSS, change database config"
```

**Good:**

```bash
git commit -m "Fix login bug"
git commit -m "Add footer component"
git commit -m "Update button styles"
git commit -m "Change database config"
```

If one change breaks something, you can undo just that commit without losing everything else.

## 28. Write Clear Commit Messages

- Describe **what** you did, not how.
- Keep it short but specific.
- Start with a verb: "Add", "Fix", "Update", "Remove", "Refactor".

## 29. Git Syntax Pattern

Every Git command follows this pattern:

```
git  <action>  <destination>
```

| Command                         | action      | destination                    |
|---------------------------------|-------------|--------------------------------|
| `git add .`                     | add         | `.` (all files)                |
| `git commit -m "message"`       | commit -m   | `"message"`                    |
| `git push origin main`          | push        | `origin main`                  |
| `git clone <url>`               | clone       | `<url>`                        |
| `git status`                    | status      | (none)                         |

---

# 🚨 PART 9 — COMMON ERRORS

---

## 30. Error Solutions

### "Support for password authentication was removed"

You cloned with HTTPS instead of SSH. Fix:

```bash
git remote set-url origin git@github.com:USER-NAME/REPOSITORY-NAME.git
```

### "upstream is gone"

Normal. Happens when your cloned repo has no branches yet. Goes away after your first push.

### "Your branch is ahead of 'origin/main' by X commits"

You have commits that haven't been pushed. Run `git push`.

### "command not found: code"

VS Code is not in your PATH. Open VS Code → `Cmd + Shift + P` → type "Shell Command" → click **"Install 'code' command in PATH"**.

### Screen stuck on `(END)`

You're in the `git log` pager. Press **`q`** to exit.

---

# 📋 PART 10 — QUICK REFERENCE TABLE

---

### Daily Use (Most Important)

| Command | What it does |
|---------|-------------|
| `git status` | See what changed / staged / committed |
| `git add <file>` | Stage one file |
| `git add .` | Stage all changed files |
| `git commit -m "message"` | Save a snapshot with a message |
| `git push` | Upload commits to GitHub |
| `git log` | View commit history |
| `git log --oneline` | Compact commit history |

### Fixing Mistakes

| Command | What it does |
|---------|-------------|
| `git restore <file>` | Undo changes (before staging) |
| `git restore --staged <file>` | Unstage a file (keep edits) |
| `git reset --soft HEAD~1` | Undo last commit (keep changes) |
| `git reset --hard HEAD~1` | Undo last commit (delete everything) |
| `git diff` | See changes before staging |
| `git diff --staged` | See changes after staging |

### Files & Folders

| Command | What it does |
|---------|-------------|
| `git mv old new` | Rename a file or folder |
| `git rm <file>` | Delete a file and stage it |
| `git rm --cached <file>` | Stop tracking but keep on disk |

### Branches & Collaboration

| Command | What it does |
|---------|-------------|
| `git branch` | List all branches |
| `git switch <name>` | Switch to a branch |
| `git switch -c <name>` | Create and switch to a branch |
| `git merge <branch>` | Merge a branch into current |
| `git branch -d <name>` | Delete a branch |
| `git pull` | Download updates from GitHub |
| `git stash` | Save work temporarily |
| `git stash pop` | Restore stashed work |
| `git blame <file>` | See who changed each line |

### Setup (One Time)

| Command | What it does |
|---------|-------------|
| `git --version` | Check Git version |
| `git config --global user.name "Name"` | Set your name |
| `git config --global user.email "email"` | Set your email |
| `git config --global init.defaultBranch main` | Default branch = main |
| `git config --global core.editor "code --wait"` | Use VS Code for messages |
| `git config --list` | View all settings |
| `git clone <SSH-URL>` | Download a repo from GitHub |
| `git remote -v` | Show remote details |
| `git remote set-url origin <URL>` | Switch HTTPS to SSH |

---

> **Remember:** `git status` is your best friend. When in doubt, run it.