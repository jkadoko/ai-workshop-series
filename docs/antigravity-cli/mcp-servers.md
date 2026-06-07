# MCP Servers & Tool Integration

## Overview

In a hospital, not every staff member has access to every system. A nurse can access patient vitals but not the pharmacy inventory. A pharmacist can dispense medications but cannot modify surgical schedules. Each role has **scoped permissions** that grant access only to the tools and data needed for their specific job.

The **Model Context Protocol (MCP)** works the same way for AI agents. MCP is an open standard that defines how AI models safely and securely access external tools—databases, APIs, file systems, web services—while maintaining strict boundaries on what they can and cannot do.

Without MCP, an AI agent would either have no access to external data (useless for real work) or unrestricted access to everything (a massive security and compliance risk). MCP provides the middle ground: **controlled, auditable tool access**.

> **Why This Matters in Regulated Industries:**
> When an AI agent accesses a Quality Management System (QMS), reads a design specification, or queries a regulatory database, that access must be:
> - **Authorized** — only pre-approved tools are available
> - **Scoped** — the tool can only read/write what it is explicitly permitted to
> - **Auditable** — every tool invocation is logged
> - **Documented** — the tool's capabilities and limitations are defined upfront

---

## Concepts

### What is an MCP Server?

An MCP server is a small program that exposes specific **tools** (functions) to the AI agent. Think of each MCP server as an "approved vendor" on your organization's qualified supplier list.

```
┌──────────────────┐     MCP Protocol     ┌──────────────────┐
│                  │ ◄──────────────────► │                  │
│   AI Agent       │    Tool Requests     │   MCP Server     │
│  (Antigravity)   │    & Responses       │   (e.g., GitHub) │
│                  │                      │                  │
└──────────────────┘                      └──────────────────┘
```

**Examples of MCP servers:**
| MCP Server | What It Provides | Use Case |
|-----------|-----------------|----------|
| **GitHub** | Issues, pull requests, code search | Track tasks, review code, manage repos |
| **Filesystem** | Read/write local files | Process documents, generate reports |
| **Firebase** | Database, authentication | Manage cloud data |
| **Custom (your own)** | Any API or database | Query your internal QMS, LIMS, etc. |

### Tools vs. Skills vs. Prompts

These three concepts work together but serve different purposes:

| Concept | What It Is | Analogy |
|---------|-----------|---------|
| **Prompt** | A single instruction to the AI | Verbal order to a technician |
| **Skill** | A reusable procedure (SOP for AI) | Written Standard Operating Procedure |
| **Tool (via MCP)** | An external capability the AI can invoke | Laboratory instrument the technician can use |

A complex agentic workflow combines all three:
- The **skill** defines the procedure
- The **prompt** triggers the workflow
- The **tools** provide the data and actions needed to execute

---

## How MCP Works in the Antigravity CLI

### Configuration

MCP servers are configured in a configuration file. In the Antigravity CLI, you can configure these globally or at a project level.

**File Locations:**
- **User-level (Global):** `~/.gemini/antigravity-cli/settings.json` (or `mcp_config.json`)
- **Project-level:** `.agent/settings.json` (or `.agent/mcp_config.json`) in your project folder

Here is an example configuration adding a GitHub MCP server:

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "<your-token>"
      }
    }
  }
}
```

**Key fields:**
- `command` — how to start the MCP server (e.g., using `npx` to fetch and run Node packages)
- `args` — the specific server package to run
- `env` — environment variables (secrets) the server needs

> ⚠️ **Security Note:** The `env` field often contains secrets (API tokens). Never commit actual tokens to version control. Keep project-level secrets in your `.env` file and use template placeholders in configuration files tracked by Git.

### Permission Model

When the AI agent wants to use a tool, the Antigravity CLI:
1. **Checks** if the tool is available (defined in the configuration file).
2. **Requests** permission from the user (interactive confirmation).
3. **Executes** the tool with scoped parameters.
4. **Returns** the result to the AI for processing.

This "ask before acting" model is the **Human-in-the-Loop (HITL)** checkpoint that regulators expect.

---

## Hands-On Exercise: Connecting the GitHub MCP Server

### Prerequisites
- Antigravity CLI installed and authenticated (Module 3)
- A GitHub account
- A GitHub Personal Access Token (PAT)

### 1. Generate a GitHub Personal Access Token

**Steps:**
1. Go to [github.com/settings/tokens](https://github.com/settings/tokens)
2. Click **"Generate new token (classic)"**
3. Give it a descriptive name (e.g., "Antigravity CLI Workshop")
4. Select scopes: `repo`, `read:org` (minimum needed)
5. Click **Generate token**
6. **Copy the token immediately** — you will not see it again.

> ⚠️ **Treat this token like a password.** Store it in your `.env` file, not in plain text in files you commit.

---

### 2. Configure the MCP Server

**Steps:**
1. Open your Antigravity settings file. The location depends on your setup:
   - **User-level**: `~/.gemini/antigravity-cli/settings.json`
   - **Project-level**: `.agent/settings.json` in your project folder
2. Add the GitHub MCP server configuration:
   ```json
   {
     "mcpServers": {
       "github": {
         "command": "npx",
         "args": ["-y", "@modelcontextprotocol/server-github"],
         "env": {
           "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_your_token_here"
         }
       }
     }
   }
   ```
3. Save the file.

---

### 3. Test the Connection

**Steps:**
1. Start (or restart) the Antigravity interactive CLI:
   ```powershell
   agy
   ```
2. Ask the AI to use the GitHub tool:
   ```
   List the 5 most recent issues from the repository microsoft/vscode.
   Format as a table with columns: Issue Number, Title, State, Created Date.
   ```
3. The CLI will ask for permission to use the GitHub tool — approve it.
4. The AI should return a formatted table with real, live data from GitHub.

---

### 4. Explore Available Tools

**Steps:**
1. Ask the AI what tools are available:
   ```
   What MCP tools do you have access to? List them with a brief description of each.
   ```
2. Try a few more GitHub-specific queries:
   ```
   Search for repositories related to "medical device software" with more than 100 stars.
   ```

---

## Building Your Own MCP Server (Preview)

For advanced users, you can create custom MCP servers that connect the AI to your organization's internal systems.

**Example: A "Design Control" MCP Server** that provides tools to:
- Query your QMS for open CAPAs
- Retrieve design specifications by document number
- Check the status of verification/validation activities

This is built using **FastMCP** (Python) or the **MCP SDK** (Node.js/TypeScript):

```python
# Example: A minimal custom MCP server (conceptual)
from fastmcp import FastMCP

mcp = FastMCP("design-control")

@mcp.tool()
def get_requirement(doc_id: str) -> str:
    """Retrieve a design requirement by document ID."""
    # In production: query your QMS database
    requirements = {
        "REQ-001": "The device shall operate for 4 hours on battery.",
        "REQ-002": "The device shall withstand a 1m drop test.",
    }
    return requirements.get(doc_id, f"Requirement {doc_id} not found.")
```

> **Note:** Building custom MCP servers is an advanced topic. This workshop focuses on using pre-built servers. Custom server development is covered in advanced training.

---

## Security Considerations for MCP

| Risk | Mitigation |
|------|-----------|
| Over-permissioned tools | Use minimum required scopes (principle of least privilege) |
| Token exposure | Store tokens in `.env`, never in configs committed to Git |
| Unauthorized actions | HITL confirmation before tool execution |
| Data exfiltration | Scope tools to read-only when write access is not needed |
| Unverified servers | Only use MCP servers from trusted sources (official packages, internal builds) |

---

## ✅ Verification Checkpoint

| Check | How to Verify | Expected Result |
|-------|--------------|----------------|
| GitHub PAT generated | Check github.com/settings/tokens | Token exists |
| MCP configured | Review settings file | GitHub server entry present |
| Connection works | Ask AI to list GitHub issues | Real data returned |
| Permission prompt | Use any MCP tool | CLI asks for approval before executing |
| Token not in Git | `git status` and review `.gitignore` | Token/settings not tracked |

You are now ready for **Module: Governance, Audit Trails & Human-in-the-Loop**.
