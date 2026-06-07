# Version Control with Git

## Overview

Imagine you are writing a critical design specification for a Class III medical device. Over the course of months, dozens of revisions are made by multiple engineers. Without a system to track these changes, you face a nightmare of conflicting versions, lost edits, and no way to prove who changed what and when.

**Git** is a version control system that solves this problem for software—and increasingly, for all engineering documents. Think of Git as your project's **Design History File (DHF)**—an immutable, chronological record of every change ever made, who made it, and why.

> **Why This Matters in Regulated Industries:**
> FDA 21 CFR Part 820 (now QMSR) and IEC 62304 require documented configuration management for software. Git provides:
> - **Traceability** — every change is linked to an author and a description
> - **Auditability** — the complete history is preserved and cannot be silently altered
> - **Rollback** — if a change introduces a problem, you can revert to any previous state
> - **Collaboration** — multiple engineers can work on the same project without overwriting each other

---

## Installation

### 1. Install Git Using Winget

**Steps:**
1. Open Windows PowerShell
2. Type the following command and press **Enter**:
   ```powershell
   winget install Git.Git
   ```
3. Follow any on-screen prompts
4. **Important:** Close and reopen PowerShell after installation

---

### 2. Verify the Installation

**Steps:**
1. In a new PowerShell window, type:
   ```powershell
   git --version
   ```
2. You should see a version number (e.g., `git version 2.47.0.windows.1`)

---

### 3. Configure Your Identity

**Justification:** Every Git "commit" (saved change) is stamped with the author's name and email. This is your digital signature on the audit trail.

**Steps:**
1. Set your name:
   ```powershell
   git config --global user.name "Your Full Name"
   ```
2. Set your email:
   ```powershell
   git config --global user.email "your.email@company.com"
   ```

---

## Core Concepts

### The Three States of a File

Git tracks files through three states:

```
Working Directory  →  Staging Area  →  Repository (History)
   (your edits)       (ready to save)    (permanent record)
```

| State | Analogy | Description |
|-------|---------|-------------|
| **Working Directory** | Lab notebook draft | Files you are actively editing |
| **Staging Area** | Review queue | Files marked as "ready to record" |
| **Repository** | Design History File | Permanently saved snapshot of your project |

### What is a "Commit"?

A **commit** is a snapshot of your project at a specific point in time. Each commit includes:
- The exact state of every tracked file
- A description (commit message) explaining what changed and why
- The author's name and timestamp
- A unique ID (hash) that can never be duplicated

Think of each commit as a signed and dated entry in a lab notebook.

---

## Hands-On Exercise

### 1. Initialize a Git Repository

**Justification:** Before Git can track changes, you must initialize it in your project folder. This creates a hidden `.git` folder that contains the entire history.

**Steps:**
1. Navigate to a project folder:
   ```powershell
   cd ~/Desktop/venv-practice
   ```
2. Initialize Git:
   ```powershell
   git init
   ```
3. You should see: `Initialized empty Git repository in ...`

---

### 2. Create a `.gitignore` File

**Justification:** Not all files should be tracked. Virtual environments (`.venv/`), secret files (`.env`), and temporary files (`__pycache__/`) must be excluded from version control. The `.gitignore` file tells Git which files to ignore.

**Steps:**
1. Create the file:
   ```powershell
   New-Item .gitignore
   ```
2. Open it in Notepad (or VS Code):
   ```powershell
   notepad .gitignore
   ```
3. Add the following lines:
   ```
   # Virtual environment
   .venv/

   # Environment variables (secrets)
   .env
   .env.local

   # Python cache
   __pycache__/
   *.pyc

   # OS files
   Thumbs.db
   .DS_Store
   ```
4. Save and close

> ⚠️ **Critical Rule:** Never commit secrets (API keys, passwords, tokens) to Git. Once a secret enters Git history, it is extremely difficult to remove and should be considered compromised.

---

### 3. Check the Status

**Steps:**
1. Run:
   ```powershell
   git status
   ```
2. You will see a list of "untracked files"—files Git knows about but is not yet recording

---

### 4. Stage Your Files

**Justification:** Staging is like placing items in a review queue before officially recording them. This two-step process (stage → commit) prevents accidental recordings.

**Steps:**
1. Add all files to the staging area:
   ```powershell
   git add .
   ```
   The `.` means "everything in the current directory" (except items in `.gitignore`)
2. Check the status again:
   ```powershell
   git status
   ```
   Files should now appear in green under "Changes to be committed"

---

### 5. Make Your First Commit

**Justification:** This is the moment you create a permanent record. The commit message should clearly describe what changed and why—just like an entry in a controlled document.

**Steps:**
1. Create the commit:
   ```powershell
   git commit -m "Initial project setup with .gitignore and requirements.txt"
   ```
2. You should see a summary showing how many files were changed and inserted

> **Best Practice for Commit Messages:**
> - Start with a verb: "Add", "Fix", "Update", "Remove"
> - Be specific: "Add pandas dependency" not "Updated stuff"
> - In regulated contexts, reference ticket/issue numbers: "Fix #42: Correct sensor calibration threshold"

---

### 6. View the History

**Steps:**
1. View the commit log:
   ```powershell
   git log --oneline
   ```
2. You will see your commit with its unique hash and message:
   ```
   a1b2c3d Initial project setup with .gitignore and requirements.txt
   ```

---

### 7. Make a Change and Commit Again

**Steps:**
1. Edit any file (e.g., add a comment to `hello_vscode.py`)
2. Stage and commit:
   ```powershell
   git add .
   git commit -m "Add documentation comment to hello_vscode.py"
   ```
3. View the updated log:
   ```powershell
   git log --oneline
   ```
   You now have two entries in your Design History File.

---

### 8. View What Changed (Diff)

**Justification:** Before committing, you should always review what changed. This is the equivalent of a peer review before signing a document.

**Steps:**
1. Make a small edit to any file (but do NOT stage it yet)
2. View the changes:
   ```powershell
   git diff
   ```
3. Lines prefixed with `+` are additions; lines prefixed with `-` are deletions
4. If satisfied, stage and commit as before

---

## The Git Workflow (Summary)

```
Edit files → git add . → git commit -m "Description" → Repeat
              (stage)        (record)
```

Every cycle adds a new entry to your permanent, auditable history.

---

## ✅ Verification Checkpoint

| Check | How to Verify | Expected Result |
|-------|--------------|----------------|
| Git installed | `git --version` | Version number displayed |
| Identity configured | `git config --global user.name` | Your name displayed |
| Repo initialized | `git status` in project folder | No "fatal: not a git repository" error |
| `.gitignore` works | Add `.venv/` to `.gitignore`, run `git status` | `.venv` folder is NOT listed |
| Commit created | `git log --oneline` | At least one commit entry |
| History is immutable | `git log` | Author, date, hash, and message for each commit |

---

## Key Takeaways

| Git Command | Purpose | Regulated Industry Analogy |
|-------------|---------|---------------------------|
| `git init` | Start tracking a project | Open a new DHF |
| `git add` | Mark files for recording | Queue for review |
| `git commit` | Create a permanent record | Sign and date an entry |
| `git log` | View complete history | Read the DHF |
| `git diff` | See what changed | Red-line markup |
| `.gitignore` | Exclude sensitive/temp files | Controlled access list |

You are now ready for **Module: Secrets & Environment Variables**.
