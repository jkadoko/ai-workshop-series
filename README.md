# AI & Automation Workshop Series

*Hands-on training curriculum for working professionals in regulated industries.*

[![Build Status](https://img.shields.io/github/actions/workflow/status/jkadoko/ai-workshop-series/build-test.yaml?style=flat-square&label=Build)](https://github.com/jkadoko/ai-workshop-series/actions)
![Node version](https://img.shields.io/badge/Node.js->=20-3c873a?style=flat-square)
![Python version](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat-square&logo=python&logoColor=white)

---

This repository contains hands-on training materials designed to bridge the gap between core programming, command-line interfaces, and advanced AI automation. Tailored specifically for engineers and scientists in highly regulated fields (medical devices, biotechnology, pharmaceuticals), the curriculum focuses on reproducibility, compliance, governance, and standardized workflows.

[Overview](#overview) • [Learning Path](#learning-path) • [Workshop Modules](#workshop-modules) • [Repository Structure](#repository-structure) • [Getting Started](#getting-started)

---

## Overview

In regulated industries such as medical devices and biotechnology, documentation, auditability, and safety are paramount. General-purpose AI tools can be unpredictable and hard to verify. This workshop series teaches professionals how to build, run, and control AI-assisted workflows using deterministic logic, structured templates, and standardized procedures (Agent Skills).

Students progress from zero experience to building audit-ready, AI-assisted workflows with human-in-the-loop governance—the kind of workflows that satisfy FDA, EU AI Act, and ISO requirements.

> [!NOTE]
> All workshop instructions are optimized for Windows laptops and utilize tools like PowerShell, the Microsoft Store, VS Code, and the Antigravity CLI to ensure a smooth, secure environment configuration.

---

## Learning Path

```
Zero ──► Environment Setup ──► Python Essentials ──► Agentic Workflows ──► Governance & Capstone ──► Hero
```

| Phase | What You Learn | Outcome |
|-------|---------------|---------|
| **Environment Setup** | Python, virtual envs, VS Code, Git, secrets | Professional development workstation |
| **Python Essentials** | Package management, reproducible builds | Can install and document dependencies |
| **Agentic Workflows** | Antigravity platform, prompt engineering, skills, MCP | Can build structured AI-assisted workflows |
| **Governance & Capstone** | HITL, audit trails, design verification | Can deploy AI responsibly in regulated settings |

---

## Workshop Modules

The training is structured into four sequential sections with 13 modules:

### 🔧 Section 0: Environment Setup

These foundational modules ensure every student has a professional-grade development environment before touching AI.

#### Module 0.1: Python Installation
Learn to configure a verified developer environment. Covers installing Python via the Microsoft Store, verifying system path configurations, and executing your first script.
* 📖 **Guide**: [docs/setup-python/install-python.md](docs/setup-python/install-python.md)

#### Module 0.2: Virtual Environments
Isolate project dependencies for reproducibility and compliance. Learn the "sterile field" approach to software development.
* 📖 **Guide**: [docs/environment-setup/virtual-environments.md](docs/environment-setup/virtual-environments.md)

#### Module 0.3: IDE Setup — VS Code
Upgrade from Notepad to a professional development environment with syntax highlighting, integrated terminal, and Git support.
* 📖 **Guide**: [docs/environment-setup/install-vscode.md](docs/environment-setup/install-vscode.md)

#### Module 0.4: Version Control with Git
Track every change with an immutable audit trail. Learn Git as your project's Design History File (DHF).
* 📖 **Guide**: [docs/environment-setup/git-basics.md](docs/environment-setup/git-basics.md)

#### Module 0.5: Secrets & Environment Variables
Manage API keys, credentials, and tokens safely. Learn the "never commit secrets" rule and `.env` workflows.
* 📖 **Guide**: [docs/environment-setup/secrets-management.md](docs/environment-setup/secrets-management.md)

---

### 🐍 Section 1: Python Essentials

#### Module 1.1: Python Package Management (pip)
Extend Python's capabilities with third-party libraries. Install packages in virtual environments and document dependencies with `requirements.txt`.
* 📖 **Guide**: [docs/install-packages/pip-install.md](docs/install-packages/pip-install.md)

---

### 🤖 Section 2: Command-Line AI & Agentic Workflows with Antigravity

#### Module 2.1: Introduction to Agents & Platform Setup (IDE & Agent Surface)
Understand core agentic concepts, benefits, limitations, and setup Google's agentic platform. Configure the VS Code integration (IDE) and the Desktop App/Agent Manager surface.
* 📖 **Guides**: [docs/setup-antigravity/intro-to-agents.md](docs/setup-antigravity/intro-to-agents.md), [docs/setup-antigravity/setup-ide.md](docs/setup-antigravity/setup-ide.md), & [docs/setup-antigravity/setup-agent-surface.md](docs/setup-antigravity/setup-agent-surface.md)

#### Module 2.2: Antigravity CLI Setup & Programmatic Interaction
Bridge the gap between your local terminal and large language models using the Go-based CLI (`agy`). Learn installation, authentication, and the programmatic advantages of terminal-first workflows (scriptability, CI/CD, batch processing).
* 📖 **Guide**: [docs/antigravity-cli/setup-antigravity-cli.md](docs/antigravity-cli/setup-antigravity-cli.md)

#### Module 2.3: Prompt Engineering for Compliance
Move beyond chat to structured, audit-ready prompting. Learn output format constraints, the "explain before you execute" pattern, and PII protection.
* 📖 **Guide**: [docs/antigravity-cli/prompt-engineering.md](docs/antigravity-cli/prompt-engineering.md)

#### Module 2.4: Agentic Workflows & Skills
Go beyond basic chat. Explore how agentic systems plan, use tools, and leverage "Skills" as digital Standard Operating Procedures (SOPs) under `.agent/skills/`. Build your first skill from scratch.
* 📖 **Guide**: [docs/antigravity-cli/add-skills.md](docs/antigravity-cli/add-skills.md)

#### Module 2.5: MCP Servers & Tool Integration
Connect your AI agent to external systems (GitHub, databases, APIs) using the Model Context Protocol. Understand settings configuration and secure tool access.
* 📖 **Guide**: [docs/antigravity-cli/mcp-servers.md](docs/antigravity-cli/mcp-servers.md)

#### Module 2.6: MS Office Document Automation (Word & Excel)
Learn how AI agents read, edit, and audit binary MS Office documents (.docx and .xlsx). Focuses on pandoc Markdown conversion, raw XML editing (OOXML), openpyxl formatting, and formula recalculation validation.
* 📖 **Guide**: [docs/office-automation/office-automation.md](docs/office-automation/office-automation.md) & [docs/office-automation/docx-templates.md](docs/office-automation/docx-templates.md)

---

### 🏛️ Section 3: Governance & Capstone

#### Module 3.1: Governance, Audit Trails & Human-in-the-Loop
Design AI workflows that satisfy regulatory expectations. Learn HITL review patterns, audit trail design, and the FDA's "Human-in-Command" standard.
* 📖 **Guide**: [docs/governance/audit-trails-and-hitl.md](docs/governance/audit-trails-and-hitl.md)

#### Module 3.2: Capstone — Automated Design Verification
A comprehensive capstone exercise. Automate the verification of design requirements against a NotebookLM knowledge base using Playwright browser automation, generate compliance Word documents, and apply HITL governance to the outputs.
* 📖 **Guide**: [docs/antigravity-cli/use-skills.md](docs/antigravity-cli/use-skills.md)

#### Module 3.3: Next Steps & Continuous Learning
Your roadmap from individual practitioner to team leader. Covers enterprise governance, custom MCP servers, regulatory resources, and recommended learning paths.
* 📖 **Guide**: [docs/governance/next-steps.md](docs/governance/next-steps.md)

---

## Repository Structure

```text
├── .agent/            # Agent operational context, specifications, rules, and skills
│   └── skills/        # Example skills (design-review-checklist)
├── data/              # Sample datasets and requirement templates
│   └── sample-requirements.csv  # Medical device requirements for capstone
├── docs/              # Detailed workshop modules and documentation
│   ├── environment-setup/   # VS Code, Git, virtual envs, secrets management
│   ├── antigravity-cli/     # CLI setup, prompt engineering, skills, MCP, capstone
│   ├── setup-antigravity/   # Agent concepts, IDE and Agent Surface setup
│   ├── office-automation/   # Word/Excel templating and programmatic population
│   ├── governance/          # HITL, audit trails, next steps
│   └── install-packages/    # Python installation and package management
├── src/               # Shared logic and application scripts
├── tests/             # Quality control unit tests
└── README.md          # Master training index and roadmap
```

---

## Getting Started

To prepare your laptop for the workshops:

1. **Verify Git & Clone Repository:**
   Ensure you have Git installed on your system, then clone this repository to your local workspace.
   ```powershell
   git clone https://github.com/jkadoko/ai-workshop-series.git
   cd ai-workshop-series
   ```

2. **Configure Environment:**
   Copy the example environment file and add your credentials:
   ```powershell
   copy .env.example .env
   ```

3. **Follow the Modules in Order:**
   Start with [Module 0.1: Python Installation](docs/setup-python/install-python.md) and work through each module sequentially. Each module builds on the previous one.

---

## Key Principles

This curriculum is built on five principles essential for regulated industry AI adoption:

| Principle | How We Teach It |
|-----------|----------------|
| **Reproducibility** | Virtual environments, `requirements.txt`, documented configurations |
| **Traceability** | Git version control as a Design History File |
| **Standardization** | Skills (SOPs for AI) ensure consistent, repeatable outputs |
| **Human Oversight** | Human-in-the-Loop review patterns for all AI-generated outputs |
| **Security** | Secrets management, `.gitignore`, scoped MCP permissions |
