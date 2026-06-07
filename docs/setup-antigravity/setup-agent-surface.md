# Setting Up the Antigravity Agent Surface (Desktop App)

## Overview

While the **Antigravity IDE** is designed for interactive, line-by-line coding, and the **Antigravity CLI** is optimized for programmatic scripting, the **Antigravity Desktop Application** provides a dedicated graphical environment for orchestrating autonomous agentic workflows. 

The core of this application is the **Agent Manager** surface. In a professional workflow, a task is rarely a simple single-turn question. It often requires:
- Generating and testing code across several files in parallel
- Executing long-running search or research tasks that take minutes or hours
- Spawning specialized subagents (e.g., a "Security Auditor" subagent and a "TDD Test Writer" subagent) and coordinating their work

The Desktop App and Agent Manager give you a bird's-eye view of all running, queued, and completed agentic processes.

```
┌──────────────────────────────────────────────────────────────────┐
│ Antigravity Desktop App (Agent Manager Surface)                   │
│                                                                  │
│  [ + New Agent Task ]  [ Running Tasks (2) ]  [ History ]        │
│                                                                  │
│  ┌────────────────────────────────────────────────────────────┐  │
│  │ ⏳ Task: Run Design Verification Capstone                  │  │
│  │    ├─ 📄 Main Agent: Planning & coordination (Active)       │  │
│  │    ├─ 🤖 Subagent 1: Playwright Browser Fetch (Completed)   │  │
│  │    └─ 🤖 Subagent 2: Document Builder (Active)              │  │
│  └────────────────────────────────────────────────────────────┘  │
│  ┌────────────────────────────────────────────────────────────┐  │
│  │ 🟢 Task: Security Audit on Local Secrets                    │  │
│  │    └─ 🤖 Main Agent: Checking .env vs git history (Idle)    │  │
│  └────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────┘
```

### Why Use the Desktop / Agent Manager Surface?

- **Parallel Orchestration** — Run and monitor multiple agents simultaneously without blocking your terminal or editor workspace.
- **Asynchronous Execution** — Launch a complex process (like a full project audit) and let it run in the background while you focus on other work.
- **Visual Log Viewer** — Inspect execution steps, see which tools were called, and review exact inputs and outputs for auditing.
- **Human-in-the-Loop Gatekeeping** — Visual prompts alert you when an agent is waiting for approval to run a command or write a file, making HITL compliance seamless.

> **In a Regulated Context:** This surface is your control room. It provides a visual, real-time audit trail of every decision the agent makes, which tool it uses, and where it is requesting human approval. This is critical for demonstrating that the "Human-in-Command" standard is being maintained.

---

## Installation & Setup

### 1. Download and Install the App

**Justification:** The standalone desktop application runs locally on your Windows machine and contains the core agent orchestration engine.

**Steps:**
1. Open Windows PowerShell.
2. We can download and install the Antigravity Desktop client using `winget` to ensure we get a verified, secure package:
   ```powershell
   winget install Google.Antigravity
   ```
3. Follow the installation wizard prompts.
4. Launch the application from your Windows **Start menu** by typing **Antigravity** and pressing **Enter**.

---

### 2. Connect Your Workspace

**Justification:** The desktop application needs to be pointed to your local project files to perform operations, just like VS Code.

**Steps:**
1. On the Antigravity welcome screen, click **Open Workspace Folder**.
2. Navigate to your project directory (e.g., `C:\Users\Jonah\Documents\projects\ai-workshop-series`) and click **Select Folder**.
3. The app will open the workspace dashboard, showing active files, local configurations, and any existing `.agent` directory.

---

## Key Agent Manager Workflows

### 1. Launching a Background Agent Task

When you have a multi-step task, you can delegate it to a background agent.

**Steps:**
1. Click the **+ New Agent Task** button in the top-left of the Desktop app.
2. Give the task a title: `Requirements Document Compliance Check`.
3. In the task prompt area, enter:
   > Scan the markdown files in docs/ and verify that all external URL links are active and functional. Do not modify the files; just output a summary report.
4. Click **Start Task**.
5. The task will appear in the **Active Tasks** panel. You can watch the agent create a plan, execute web check tools, and output its findings.

---

### 2. Handling Permission Requests (Human-in-the-Loop)

By default, the Antigravity agent runs in a sandboxed, secure environment. It cannot read arbitrary directories, write files, or execute terminal commands without your permission.

**Steps:**
1. When a running agent needs to execute a command (e.g., `git commit`) or write a file, it will pause.
2. The Desktop App will display a **Permission Requested** notification.
3. Review the request details:
   - **Action:** e.g., Write file
   - **Target:** `C:\Users\Jonah\Documents\projects\ai-workshop-series\reports\link_audit.md`
   - **Reason:** To save the compiled list of active/broken URL references.
4. Click **Approve** to allow the operation, or **Reject** to block it.

---

### 3. Monitoring Subagent Spawning

For complex problems, the primary agent may spawn specialized subagents.

**Concept:**
- **Primary Agent** — Handles the high-level goal, schedules tasks, and communicates with the user.
- **Subagents** — Spin up to handle isolated, narrow subtasks (e.g., searching an API, compiling a specific file, or writing unit tests).

In the **Agent Manager** panel, these subagents appear as child nodes nested under the primary task. You can click on any subagent to view its specific logs, tool calls, and execution state.

---

## ✅ Verification Checkpoint

| Check | How to Verify | Expected Result |
|-------|--------------|----------------|
| Application installed | Launch "Antigravity" from Start menu | App opens successfully |
| Workspace connected | Review workspace path in title bar | Correct folder is listed |
| Task runs in background | Launch a link-checking task | Task appears in "Active Tasks" |
| HITL notification works | Check status during write operation | App pauses and prompts for approval |
| Log viewer accessible | Click on a completed task | Full list of tool executions is visible |

You are now ready to set up the **Antigravity CLI** and learn why it is crucial for programmatic workflows.
