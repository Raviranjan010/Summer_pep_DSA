# ⚙️ 01 — Git & GitHub

> **Goal:** Save your code forever, and build a daily coding habit that recruiters will love.

---

## 🌎 Why Git & GitHub? (The Scenario)

Imagine you have been working on a complex DSA project for 6 months. Late one night, your laptop screen goes black. **It won't turn on. Your hard drive crashed, and all your code is gone.**

- **Git** is a local software tool that tracks the history of changes made to your files. It acts like a "Save Game" system for code.
- **GitHub** is a free cloud-based platform where you can store your Git repositories. It is like Google Drive, but built specifically for coding projects.

By uploading (pushing) your code daily, you create a permanent, off-site backup, and build a public portfolio (your GitHub profile's "green square contribution grid") that proves your dedication to future employers.

---

## 🎨 The Git Mental Model (How it Works)

Git splits your workflow into 4 distinct areas. Understanding how files move between these areas is the key to mastering Git:

```
┌──────────────────┐     git add      ┌────────────────┐
│                  ├─────────────────►│                │
│  Working Dir     │                  │  Staging Area  │
│  (Your Files)    │◄─────────────────┤  (Ready Zone)  │
│                  │    git reset     │                │
└──────────────────┘                  └───────┬────────┘
                                              │
                                              │ git commit
                                              ▼
┌──────────────────┐     git push     ┌────────────────┐
│                  │◄─────────────────┤                │
│   GitHub Cloud   │                  │   Local Repo   │
│   (Remote Repo)  ├─────────────────►│  (Saved Zone)  │
│                  │    git fetch     │                │
└──────────────────┘                  └────────────────┘
```

1. **Working Directory:** The actual folder on your computer where you write and edit code.
2. **Staging Area:** A temporary workspace where you prepare changes for the next snapshot.
3. **Local Repository:** The `.git` database on your machine where your saved commits (snapshots) live.
4. **Remote Repository:** The hosted copy of your repository on GitHub.

---

## ⚙️ Setup Checklist

Before you can run Git commands, make sure you have the basics installed:

- [ ] **Git:** Download and install from [git-scm.com](https://git-scm.com/).
- [ ] **GitHub Account:** Register for free at [github.com](https://github.com/).
- [ ] **Language Tools:** Install the compiler/interpreter for your language of choice.

Verify installations by running these commands in your terminal:
```bash
git --version      # Check Git installation
java --version     # Java compilers
python --version   # Python interpreters
g++ --version      # C++ compilers
```

---

## 🚀 The Only 5 Git Commands You Need (For Now)

Once Git is configured, your daily routine consists of running these five commands:

```bash
git init                       # 1. Initialize a new local Git repository (Run once per project)
git status                     # 2. Check which files have been modified or are untracked
git add .                      # 3. Move all modified files into the Staging Area
git commit -m "Add arrays"     # 4. Save the staged changes as a snapshot with a message
git push                       # 5. Upload your commits to your GitHub repository
```

---

## 🛠️ First-Time Configurations

Before making your first commit, tell Git who you are. This name and email will be linked to your commits on GitHub.

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### Linking Your Local Folder to a New GitHub Repo
If you have a folder of code on your laptop and want to put it on GitHub for the first time, follow these steps:

1. Go to GitHub and click **New Repository**. Give it a name and keep it public. Do not check "Add a README" if your local folder already has files.
2. Open your terminal in your local folder and run:
```bash
git init                                                    # Create the local repository
git add .                                                   # Stage all current files
git commit -m "Initial commit"                              # Save the snapshot
git branch -M main                                          # Rename the default branch to 'main'
git remote add origin https://github.com/username/repo.git  # Connect to GitHub (replace URL)
git push -u origin main                                     # Push and link branch
```

For all future updates, you only need to run:
```bash
git add .
git commit -m "Updates for arrays"
git push
```

---

## ⚠️ Troubleshooting Tricky Situations (Beginner Pitfalls)

### 1. Error: `fatal: remote origin already exists`
This happens when you try to run `git remote add origin ...` on a folder that is already connected to a repository.
- **Solution:** Check the current connection using:
  ```bash
  git remote -v
  ```
- If the URL is wrong, change it using:
  ```bash
  git remote set-url origin https://github.com/username/correct-repo.git
  ```

### 2. You committed the wrong files but haven't pushed yet
If you accidentally ran `git commit` and want to undo it while keeping your code edits intact:
- **Solution:** Reset the commit (keeps your files modified but uncommitted):
  ```bash
  git reset --soft HEAD~1
  ```
  *(HEAD~1 means: go back exactly 1 commit)*

### 3. Merge Conflicts (When Git gets confused)
This occurs when you make changes to the same line of the same file in two different places, and Git doesn't know which version to keep.
- **Solution:**
  1. Open the conflicting file in your editor.
  2. Look for conflict markers:
     ```
     <<<<<<< HEAD
     int max = arr[0]; // Your local change
     =======
     int max_value = arr[0]; // Remote change on GitHub
     >>>>>>> main
     ```
  3. Delete the markers (`<<<<<<<`, `=======`, `>>>>>>>`) and keep the code you actually want.
  4. Save the file, run `git add .`, then `git commit -m "Fix conflict"`, and push again.

---

## 📁 Recommended Directory Structure

To keep your files clean and simple, use a nested structure:

```
DSA-Preparation/
├── .git/                      # Hidden folder containing Git database (never delete!)
├── README.md                  # Main introduction file
├── Language-Basics/           # Syntax cheat sheets
│   ├── Java-Basics.md
│   └── Python-Basics.md
├── 02-Arrays/                 # Topic folders
│   ├── Solution.java
│   └── Notes.md
└── 03-Two-Pointers/
    └── Solution.py
```


---

## 📋 Step-by-Step Dry Run: Git Workflow Lifecycle

### Scenario: Creating, Editing, Branching, and Merging

- **Initial State:** On branch `main`, working directory clean.
- **Step 1 (`git checkout -b feature-arrays`):** Creates branch `feature-arrays` and moves `HEAD` pointer to it.
- **Step 2 (`echo "solution" > solution.py`):** File `solution.py` is in **Untracked** state in Working Directory.
- **Step 3 (`git add solution.py`):** File moves to the **Staging Area** (Index). Git creates a blob object in `.git/objects`.
- **Step 4 (`git commit -m "Add array solution"`):** Git creates a commit object pointing to the staged tree object. `HEAD` moves to the new commit.
- **Step 5 (`git checkout main`):** `HEAD` moves back to `main`. `solution.py` disappears from the working tree.
- **Step 6 (`git merge feature-arrays`):** Performs a **Fast-Forward Merge** moving `main` forward to point to `feature-arrays` commit. `HEAD` is updated.

---

## 🎓 Viva Questions & Answers

### Q1: What are the 3 main states of a file in Git?
**Answer:**
1. **Working Directory (Untracked / Modified):** Files modified or created locally on disk but not yet added to Git tracking.
2. **Staging Area / Index (Staged):** Files marked for inclusion in the upcoming commit snapshot (`git add`).
3. **Repository / `.git` directory (Committed):** Data safely stored in Git's local object database (`git commit`).

### Q2: What is the difference between `git merge` and `git rebase`?
**Answer:**
- **`git merge`:** Combines changes from one branch into another by creating a new **merge commit** (preserves complete historical timeline).
- **`git rebase`:** Moves or re-applies commits from a branch on top of another base tip, creating a **linear history** (rewrites commit hashes).

### Q3: What is the difference between `git fetch` and `git pull`?
**Answer:**
- **`git fetch`:** Downloads new branches and commits from the remote repository to your local `.git` database without modifying your local working files.
- **`git pull`:** Performs `git fetch` followed immediately by `git merge` to integrate remote changes directly into your active local working branch.

### Q4: What is a "Detached HEAD" state in Git?
**Answer:**
A detached `HEAD` state occurs when `HEAD` points directly to a specific commit hash rather than a local branch reference (e.g. after running `git checkout <commit-hash>`). Any new commits created in this state won't belong to any branch and will eventually be garbage-collected unless saved into a new branch.

### Q5: How do you resolve a Merge Conflict?
**Answer:**
1. Git pauses the merge and inserts conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`) into affected files.
2. Open the file, choose desired lines/logic, and manually delete conflict marker lines.
3. Stage the resolved files using `git add <filename>`.
4. Finalize the resolution by creating the commit with `git commit`.

---

> 🏆 **Habit Goal:** Commit and push your code every single day. Even if it is just a single line of comments or a simple bug fix. Consistency is key!

