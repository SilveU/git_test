# Git Basics — Complete Reference

> Open this file any time you get stuck. Everything you need is right here.

---

## 1. First-Time Setup

Before you do anything with Git, make sure it's configured properly.

### Check your Git version

```bash
git --version
```

You need version **2.28 or later**. If your version is older, update Git first.

### Set your default branch name to `main`

```bash
git config --global init.defaultBranch main
```

GitHub changed the default branch name from `master` to `main`. This command makes sure every new repo you create locally also uses `main`.

### Set your name and email (required for commits)

```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

Git attaches this info to every commit you make. Use the same email you used for your GitHub account.

### Set VS Code as your commit message editor

```bash
git config --global core.editor "code --wait"
```

This means if you ever type `git commit` without the `-m` flag, VS Code will open instead of Vim. No output will appear after running this — that's normal.

### See all your current Git config

```bash
git config --list
```

---

## 2. Creating a Repository on GitHub

1. Go to [github.com](https://github.com).
2. Click the **`+`** button in the top-right corner → **"New repository"**.
   - On small screens, click your profile picture first, then you'll see the `+` button.
3. Name it (example: `git_test`).
4. Check **"Add a README file"** — this automatically creates a `README.md` inside the repo.
5. Click **"Create repository"**.

You now have a repository on GitHub with one file (`README.md`) in it.

---

## 3. Cloning — Getting the Repo to Your Computer

### Step 1: Get the SSH URL

On the GitHub repo page:

1. Click the green **"Code"** button.
2. Select the **SSH** tab (not HTTPS).
3. Copy the URL. It looks like this:

```
git@github.com:USER-NAME/REPOSITORY-NAME.git
```

> **WARNING:** If your URL starts with `https://`, you picked HTTPS, not SSH. Go back and click SSH.

### Step 2: Create a folder for your projects (one time only)

```bash
cd ~
mkdir repos
cd repos
```

| Command      | What it does                                        |
|--------------|-----------------------------------------------------|
| `cd ~`       | Go to your home directory                           |
| `mkdir repos`| Create a new folder called `repos`                  |
| `cd repos`   | Move into that folder                               |

### Step 3: Clone

```bash
git clone git@github.com:USER-NAME/REPOSITORY-NAME.git
```

This downloads the entire repository into a new folder inside `repos/`.

### Step 4: Verify the connection

```bash
cd git_test
git remote -v
```

You should see:

```
origin  git@github.com:USER-NAME/git_test.git (fetch)
origin  git@github.com:USER-NAME/git_test.git (push)
```

- **`origin`** = the default name Git gives to the remote repository you cloned from. It's just a nickname. You could rename it to anything, but `origin` is the convention.
- **`(fetch)`** = the URL Git uses to download changes.
- **`(push)`** = the URL Git uses to upload changes.

---

## 4. The Two-Stage System (Staging + Committing)

Git does NOT save your work automatically. You have to go through two steps:

```
Working Directory  →  Staging Area  →  Repository (commit history)
       ↑                   ↑                    ↑
  You edit files      git add           git commit
```

1. **Working Directory** — Your actual files on disk. You edit them here.
2. **Staging Area** — A "waiting room." You pick which changes you want to include in the next commit.
3. **Commit** — A permanent snapshot saved in Git's history.

**Why two steps?** Because you might change 10 files but only want to commit 3 of them right now. The staging area lets you choose.

---

## 5. The Daily Workflow — Step by Step

### 5.1 Create or edit a file

```bash
touch hello_world.txt
```

This creates an empty file called `hello_world.txt`.

Or open an existing file and edit it:

```bash
code .
```

This opens the current folder in VS Code. Edit any file, save it.

### 5.2 Check what changed

```bash
git status
```

This is the most important command. Run it constantly. It tells you:

| Color / Section                        | Meaning                                      |
|----------------------------------------|----------------------------------------------|
| **Red** — "Untracked files"            | New file, Git doesn't know about it yet       |
| **Red** — "Changes not staged for commit" | Existing file was modified but not staged  |
| **Green** — "Changes to be committed"  | File is staged and ready to be committed      |
| "nothing to commit, working tree clean"| Everything is saved. You're up to date.       |

### 5.3 Stage your changes (add to the waiting room)

Stage one specific file:

```bash
git add hello_world.txt
```

Stage everything at once:

```bash
git add .
```

The `.` means "everything in the current directory and all subdirectories."

Run `git status` again after adding — the file(s) should now appear in **green**.

### 5.4 Commit (save the snapshot)

```bash
git commit -m "Add hello_world.txt"
```

- `-m` = message flag. The text in quotes is your commit message.
- The commit message should describe **what you did**, not how you did it.

If you forget `-m`:

```bash
git commit
```

VS Code opens (if you set it up in Section 1). Write your message, save the file (`Cmd + S`), close the tab. The commit is made.

Run `git status` after committing:

```
nothing to commit, working tree clean
```

This means the commit was successful.

### 5.5 Check your commit history

```bash
git log
```

Shows all your commits from newest to oldest. Each entry shows:

- **Commit hash** — a long unique ID (like `a1b2c3d...`)
- **Author** — who made the commit
- **Date** — when it was made
- **Message** — your commit message

If the screen gets stuck and shows `(END)` at the bottom, press **`q`** to exit.

To see a shorter version:

```bash
git log --oneline
```

### 5.6 Push to GitHub (upload your work)

```bash
git push
```

Or the full version:

```bash
git push origin main
```

| Part     | Meaning                                           |
|----------|---------------------------------------------------|
| `git push` | The command to upload                           |
| `origin`   | The name of the remote (where to push to)      |
| `main`     | The branch you're pushing                       |

If you only have one remote and one branch, `git push` is enough.

After pushing, run `git status`:

```
Your branch is up to date with 'origin/main'. nothing to commit, working tree clean
```

Refresh your GitHub repo page — your files are there now.

---

## 6. Complete Workflow Example

Here's the entire process from start to finish:

```bash
# 1. Create a file
touch hello_world.txt

# 2. Check status (file is red / untracked)
git status

# 3. Stage the file (move it to the waiting room)
git add hello_world.txt

# 4. Check status again (file is green / staged)
git status

# 5. Commit (save the snapshot)
git commit -m "Add hello_world.txt"

# 6. Check status (nothing to commit)
git status

# 7. Edit README.md — add some text, save it

# 8. Edit hello_world.txt — add some text, save it

# 9. Stage everything
git add .

# 10. Commit with a message
git commit -m "Edit README.md and hello_world.txt"

# 11. Push to GitHub
git push

# 12. Done. Check status to confirm.
git status
```

---

## 7. Important Rules

### Never edit files directly on GitHub

Always edit files on your local machine, then commit and push. If you edit on GitHub directly, your local copy and the GitHub copy will be out of sync, and fixing that requires advanced Git knowledge.

### Make atomic commits

An **atomic commit** = one commit for one thing.

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

Why? If one change breaks something, you can undo just that one commit without losing everything else.

### Write clear commit messages

- Describe **what** you did, not **how**.
- Keep it short but specific.
- Start with a verb: "Add", "Fix", "Update", "Remove", "Refactor".

---

## 8. Common Errors and What They Mean

### "Support for password authentication was removed"

You cloned with HTTPS instead of SSH. Fix it:

```bash
git remote set-url origin git@github.com:USER-NAME/REPOSITORY-NAME.git
```

Then try `git push` again.

### "upstream is gone"

Normal. This happens when your cloned repo has no branches yet. It goes away after you push for the first time.

### "Your branch is ahead of 'origin/main' by X commits"

You have commits locally that haven't been pushed yet. Run `git push`.

### "command not found: code"

VS Code is not in your PATH. Open VS Code → press `Cmd + Shift + P` → type "Shell Command" → click **"Install 'code' command in PATH"**.

### Screen stuck on `(END)`

You're in the `git log` pager. Press **`q`** to exit.

---

## 9. Git Syntax Pattern

Every Git command follows this pattern:

```
git  <action>  <destination>
```

| Command                         | action      | destination                    |
|---------------------------------|-------------|--------------------------------|
| `git add .`                     | add         | `.` (all files)                |
| `git add hello_world.txt`       | add         | `hello_world.txt`              |
| `git commit -m "message"`       | commit -m   | `"message"`                    |
| `git push origin main`          | push        | `origin main`                  |
| `git clone <url>`               | clone       | `<url>`                        |
| `git status`                    | status      | (none)                         |
| `git log`                       | log         | (none)                         |

---

## 10. Quick Command Reference

| Command | What it does |
|---------|-------------|
| `git --version` | Check installed Git version |
| `git config --global user.name "Name"` | Set your name for commits |
| `git config --global user.email "email"` | Set your email for commits |
| `git config --global init.defaultBranch main` | Set default branch to main |
| `git config --global core.editor "code --wait"` | Use VS Code for commit messages |
| `git config --list` | View all your Git settings |
| `git clone <SSH-URL>` | Download a repo from GitHub |
| `git remote -v` | Show remote connection details |
| `git status` | See what files changed / staged / committed |
| `git add <file>` | Stage one file |
| `git add .` | Stage all changed files |
| `git commit -m "message"` | Commit staged files with a message |
| `git commit` | Commit and write message in VS Code |
| `git log` | View full commit history |
| `git log --oneline` | View compact commit history |
| `git push` | Upload commits to GitHub |
| `git push origin main` | Upload to `origin` remote, `main` branch |
| `git remote set-url origin <SSH-URL>` | Switch from HTTPS to SSH |

---

> **Remember:** `git status` is your best friend. When in doubt, run it.