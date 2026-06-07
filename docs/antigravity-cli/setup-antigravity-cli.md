# Setting Up the Antigravity CLI

## Welcome

Welcome to the world of command-line AI! The **Antigravity CLI** is a powerful, lightweight, terminal-first interface built in Go that brings Google's advanced agentic capabilities directly to your computer's terminal.

Since you work in a regulated industry (such as medical devices, biotechnology, or pharmaceuticals), think of the terminal (or command prompt) as the "operating room" of your computer. While graphical interfaces (like the Desktop App or VS Code sidebar) are excellent for visual and interactive coding, the CLI is your tool for high-velocity, repeatable, and programmatic workflows.

---

## Why Use a CLI for Agentic Workflows?

Regulated engineering teams favor command-line interfaces for several reasons:

1. **Automation & Scripting:** The CLI can be easily wrapped inside PowerShell scripts, batch files, or Python scripts. You can build automation chains that run multiple tasks sequentially without human intervention (e.g., verifying 50 design requirements overnight).
2. **Headless / Unattended Execution:** You can run the CLI in environments that do not have a graphical interface, such as Continuous Integration / Continuous Deployment (CI/CD) pipelines (e.g., GitHub Actions) to run automated compliance checks or code audits on every code commit.
3. **High Velocity:** Experienced keyboard-centric developers can execute commands, query repositories, and trigger agents in milliseconds, without switching between mouse and keyboard.
4. **Pipelining (Piping):** You can feed the output of one command directly as the input to the agent. For example:
   ```powershell
   git diff | agy query "Review these code changes for security vulnerabilities"
   ```
5. **Deterministic Audit Trails:** Every CLI command run, standard input, and output response is recorded in your terminal logs or can be redirected to output text files, creating a perfect, unalterable trail of AI activities.

---

## Prerequisites Checklist

Before starting this module, confirm you have completed the following:

| Prerequisite | How to Verify | Module |
|-------------|--------------|--------|
| ✅ Python installed | `python --version` | Python Installation |
| ✅ Virtual environments | Can create and activate `.venv` | Virtual Environments |
| ✅ VS Code installed | `code --version` | IDE Setup |
| ✅ Git installed | `git --version` | Git Basics |
| ✅ Secrets management | `.env` and `.gitignore` configured | Secrets Management |

If any check fails, go back to the relevant module before proceeding.

---

## Installation Steps

Unlike older Node.js-based tools, the Antigravity CLI is a native executable written in Go, which means it runs directly on your machine without requiring NVM or a Node installation.

### 1. Open Windows PowerShell

1. Click your Windows **Start menu**.
2. Type **PowerShell**.
3. Click on **Windows PowerShell**. You will see a dark window with a blinking cursor.

---

### 1b. Configure Script Execution Policy (Troubleshooting Script Blocking)

**Justification:** By default, Windows blocks the execution of downloaded scripts to protect your computer. If you try to install the CLI or run other command-line scripts, Windows may show an error: *"Running scripts is disabled on this system."* We need to adjust this permission.

**Steps:**
1. In your PowerShell window, type the following command and press **Enter**:
   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
   ```
2. PowerShell will ask for confirmation. Type `Y` (for Yes) and press **Enter**.
3. This allows you to run locally created scripts and signed scripts downloaded from the internet.

---

### 2. Download and Install the CLI

**Justification:** We use the official Google installer script. It downloads the latest stable Go binary compiled for Windows and adds it to your system PATH so it can be run from any folder.

**Steps:**
1. In your PowerShell window, copy and paste the following command, then press **Enter**:
   ```powershell
   irm https://antigravity.google/cli/install.ps1 | iex
   ```
   *(Note: `irm` stands for Invoke-RestMethod and `iex` stands for Invoke-Expression).*
2. Wait for the download and installation to complete.
3. **Important:** Once it finishes, close your PowerShell window completely and open a new one. This refreshes the system environment variables so it recognizes the newly installed command.

---

### 3. Launch and Authenticate

**Justification:** The software is installed, but it needs to verify your Google developer account to manage rate limits and free quotas.

**Steps:**
1. In your new PowerShell window, type the following startup command and press **Enter**:
   ```powershell
   agy
   ```
2. The CLI will detect it is your first launch and initiate authentication.
3. A web browser window should open automatically. (If it doesn't, copy the URL displayed in the terminal and paste it into your browser).
4. Choose **Sign in with Google** and select your account.
5. Follow the browser prompts to authorize the CLI. Once successful, close the browser window and return to your PowerShell terminal.
6. The CLI will prompt you to choose a visual theme. Select your preferred style using the arrow keys and press **Enter**.

---

### 4. Submit Your First Prompt

**Justification:** Verify the connection works by sending an interactive prompt.

**Steps:**
1. You are now inside the interactive Antigravity CLI shell (indicated by a custom prompt like `agy>`).
2. Type the following and press **Enter**:
   ```
   Explain the importance of FDA Class II device regulations in simple terms.
   ```
3. The AI will process your request and stream the response directly in your terminal.
4. To exit the interactive shell, type `/quit` or press `Ctrl+C`.

---

### 5. Running One-Off Programmatic Queries

You do not need to enter the interactive shell to use the CLI. You can pass queries directly from PowerShell, which is key for automation.

**Steps:**
1. In your normal PowerShell terminal, run:
   ```powershell
   agy query "Summarize the difference between FDA 510(k) and PMA in one sentence."
   ```
2. The agent will run, print the answer, and exit back to your PowerShell command line.

---

## ✅ Verification Checkpoint

| Check | How to Verify | Expected Result |
|-------|--------------|----------------|
| CLI installed | Close/open PowerShell, run `agy version` | Displays version and build info |
| Interactive launch | Run `agy` | CLI enters interactive mode (`agy>`) |
| Authentication complete | Submit prompt inside `agy` | AI responds without auth warnings |
| Direct query works | Run `agy query "Test"` | Output prints directly to PowerShell |

You are now ready for **Module: Prompt Engineering for Compliance**.

---

## 📋 PowerShell & CLI Command Cheat Sheet

### Basic Navigation

- **Show Current Directory (Print Working Directory):**
  ```powershell
  Get-Location
  # Or simply: pwd
  ```
- **List Directory Contents (List Files):**
  ```powershell
  Get-ChildItem
  # Or simply: ls (or dir)
  ```
- **Change Directory (Navigate to folder):**
  ```powershell
  Set-Location "C:\path\to\folder"
  # Or simply: cd "C:\path\to\folder"
  ```
- **Go Up One Folder:**
  ```powershell
  cd ..
  ```

### File and Directory Management

- **Create a New Directory (Folder):**
  ```powershell
  New-Item -Path . -Name "new-folder" -ItemType "directory"
  # Or simply: mkdir "new-folder"
  ```
- **Create a New Empty File:**
  ```powershell
  New-Item -Path . -Name "file.txt" -ItemType "file"
  ```
- **Copy a File or Directory:**
  ```powershell
  Copy-Item -Path "source.txt" -Destination "copy.txt"
  # Or simply: cp "source.txt" "copy.txt"
  ```
- **Move or Rename a File:**
  ```powershell
  Move-Item -Path "old-name.txt" -Destination "new-name.txt"
  # Or simply: mv "old-name.txt" "new-name.txt"
  ```
- **Delete (Remove) a File or Directory:**
  ```powershell
  Remove-Item -Path "file.txt" -Force
  # Or simply: rm "file.txt"
  ```

### Antigravity CLI Commands

- **Launch Interactive Mode:**
  ```powershell
  agy
  ```
- **Run a Direct Query (One-off):**
  ```powershell
  agy query "Your question here"
  ```
- **Exit Interactive Shell:**
  Type `/quit` or press `Ctrl+C`.
- **View Configuration Menu (Inside interactive mode):**
  Type `/config` or `/settings`.

---

*Tip: When you want to see all available flags and commands, run `agy --help` from PowerShell.*
