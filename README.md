# AI & Automation Workshop Series

*Hands-on training curriculum for working professionals in regulated industries.*

[![Build Status](https://img.shields.io/github/actions/workflow/status/<username>/ai-workshop-series/build-test.yaml?style=flat-square&label=Build)](https://github.com/<username>/ai-workshop-series/actions)
![Node version](https://img.shields.io/badge/Node.js->=20-3c873a?style=flat-square)
![Python version](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat-square&logo=python&logoColor=white)

---

This repository contains hands-on training materials designed to bridge the gap between core programming, command-line interfaces, and advanced AI automation. Tailored specifically for engineers and scientists in highly regulated fields, the curriculum focuses on reproducibility, compliance, and standardized workflows.

[Overview](#overview) • [Workshop Modules](#workshop-modules) • [Repository Structure](#repository-structure) • [Getting Started](#getting-started)

---

## Overview

In regulated industries such as medical devices and biotechnology, documentation, auditability, and safety are paramount. General-purpose AI tools can be unpredictable and hard to verify. This workshop series teaches professionals how to build, run, and control AI-assisted workflows using deterministic logic, structured templates, and standardized procedures (Agent Skills).

> [!NOTE]
> All workshop instructions are optimized for Windows laptops and utilize tools like PowerShell, the Microsoft Store, and the Gemini CLI to ensure a smooth, secure environment configuration.

---

## Workshop Modules

The training is structured into five sequential modules:

### 🐍 Section 1: Python & Package Essentials

#### Module 1: Python Installation
Learn to configure a verified developer environment. This module covers installing Python via the Microsoft Store, verifying system path configurations, and executing your first script.
* 📖 **Guide**: [docs/install-packages/install-python](docs/install-packages/install-python)

#### Module 2: Python Package Management (pip)
Extend Python's baseline capabilities with third-party libraries. This module covers using `pip` (Pip Installs Packages) to fetch and verify the Pandas data analysis library.
* 📖 **Guide**: [docs/install-packages/pip-install](docs/install-packages/pip-install)

---

### 🤖 Section 2: Command-Line AI & Agentic Workflows

#### Module 3: Gemini CLI Setup & Interaction
Bridge the gap between your local terminal and large language models. Learn to install, authenticate, and prompt Google's Gemini models directly from Windows PowerShell.
* 📖 **Guide**: [docs/gemini-cli/setup-gemini-cli](docs/gemini-cli/setup-gemini-cli)

#### Module 4: Agentic Workflows & Skills
Go beyond basic chat. Explore how agentic systems plan, use tools, and leverage "Skills" as digital Standard Operating Procedures (SOPs) to ensure repeatable and compliant outcomes.
* 📖 **Guide**: [docs/gemini-cli/add-skills](docs/gemini-cli/add-skills)

#### Module 5: Automated Design Verification
A comprehensive capstone exercise. Automate the verification of design requirements against a NotebookLM knowledge base using Playwright browser automation and dynamically generate compliance Word documents.
* 📖 **Guide**: [docs/gemini-cli/use-skills](docs/gemini-cli/use-skills)

---

## Repository Structure

```text
├── .agent/            # Agent operational context, specifications, and rules
├── data/              # Sample datasets and requirement templates
├── docs/              # Detailed workshop modules and documentation
│   ├── gemini-cli/    # Guides on Gemini CLI, skills, and Playwright verification
│   └── install-packages/ # Guides on Python installation and package management
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
   git clone https://github.com/<username>/ai-workshop-series.git
   cd ai-workshop-series
   ```

2. **Configure Environment:**
   Copy the example environment file and add your credentials if needed:
   ```powershell
   copy .env.example .env
   ```

3. **Initialize the Workspace:**
   Follow the specific guides in [docs/install-packages/install-python](docs/install-packages/install-python) to set up your Python interpreter and virtual environment.
