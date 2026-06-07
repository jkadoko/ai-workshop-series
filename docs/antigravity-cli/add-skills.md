# Introduction to Agentic Workflows & Building Your First Skill

## Welcome

If you're transitioning into Artificial Intelligence (AI), you've likely used chat interfaces to ask questions and get answers. Today, we're moving beyond simple chat into **agentic workflows**—autonomous AI systems that can plan, execute tools, and achieve goals with minimal human intervention.

In this exercise, we'll explore:
- What agentic workflows are and how they work
- Why they're critical for compliance in regulated industries (medical device engineering, biotech research, etc.)
- How to build and deploy your first skill

---

## Part 1: Decoding the Terminology

Before we build anything, let's clarify the core concepts:

### Agentic Workflow
Traditional AI usage follows a simple back-and-forth conversation model. An **agentic workflow** is different: the AI is given a goal and autonomously plans, utilizes tools, and executes complex tasks to achieve it without constant human guidance.

### Agent Skills
Think of a skill as a **Standard Operating Procedure (SOP)** packaged for AI. It's a set of instructions—typically a text or markdown file—that teaches an AI agent how to perform a specific task consistently and correctly.

### Tools & Model Context Protocol (MCP)
A **tool** enables the AI to interact with the outside world (e.g., reading databases, searching the web, analyzing files). The **Model Context Protocol (MCP)** is the standard that allows AI models to safely and securely access these tools while maintaining compliance and data security.

---

## Part 2: Why Skills Matter in Regulated Industries

When working in domains requiring rigorous documentation and safety standards, you cannot afford for an AI to be "creative" with your compliance processes.

**The Problem:** Using an AI without defined skills is like hiring a brilliant intern but refusing to give them any training manuals.

**The Solution:** By defining Agent Skills, you provide rigid guardrails that ensure the AI follows standardized procedures every time. This is essential for:
- Regulatory compliance
- Audit trails
- Reproducibility
- Safety and security

---

## Part 3: Real-World Examples of Agent Skills

Industry professionals structure skills in predictable ways. To avoid writing everything from scratch, you can reference and adapt existing template libraries from trusted sources:
1. **[Anthropic Skills Repository](https://github.com/anthropics/skills)** — Contains highly detailed, production-grade skills for operations like DOCX editing, XLSX calculations, structured research, and code styling.
2. **[Awesome Copilot/Agent Skills](https://github.com/awesome-copilot)** — A community-curated collection of agent instruction templates, coding checklists, and automation profiles.

### Common Skill Types

**Document Skills**
- Force the AI to output data strictly into standardized templates
- Ensure uniformity across teams and audits
- Prevent format deviations

**Review Skills**
- Scan code, datasets, or technical writing for specific criteria
- Prevent hallucination by restricting the AI to documented information
- Enable consistent quality gates

**Tool Integration Skills (via MCP)**
- Utilize MCP servers to securely query internal Quality Management Systems (QMS)
- Read from local secure directories
- Ensure the AI only accesses authorized data sources

---

## Part 4: Hands-On Exercise — Building Your First Skill

### Understanding Skill Levels

There are **3 levels of skills**:

| Level | Scope | Priority | Use Case |
|-------|-------|----------|----------|
| **Enterprise** | Organization-wide | Highest | Org-mandated standards |
| **User** | All your projects | Medium | Personal preferences & tools |
| **Project** | Single project | Lowest | Project-specific workflows |

Enterprise skills take priority when conflicts arise. User skills apply to all projects unless overridden at the project level.

### Understanding the `SKILL.md` File

A skill is defined by a single file called `SKILL.md`. This file has two parts:

1. **YAML Frontmatter** — metadata at the top (between `---` markers) that tells the AI the skill's name and when to use it
2. **Markdown Body** — the actual instructions (the "SOP") the AI follows

Here is the anatomy of a `SKILL.md` file:

```markdown
---
name: design-review-checklist
description: >
  Generate a design review checklist from a list of requirements.
  Use when the user asks to review design inputs, create a checklist,
  or prepare for a design review meeting.
---

# Design Review Checklist Generator

## Instructions
[Step-by-step procedure for the AI to follow]

## Output Format
[Exact template the AI must use]

## Rules
[Constraints and guardrails]
```

### Step-by-Step: Create a Design Review Checklist Skill

#### 1. Create the skill directory

Skills live in a `skills` folder under the `.agent` configuration directory. For a project-level skill:

```powershell
mkdir -Force .agent\skills\design-review-checklist
```

#### 2. Create the `SKILL.md` file

Open VS Code and create the file `.agent/skills/design-review-checklist/SKILL.md` with the following content:

```markdown
---
name: design-review-checklist
description: >
  Generate a structured design review checklist from a list of
  medical device requirements. Use when the user asks to "review
  requirements", "create a design review checklist", or "prepare
  for a design review".
---

# Design Review Checklist Generator

## Purpose
This skill generates a structured design review checklist that
assesses each requirement against standard quality criteria.

## Procedure
1. Read the list of requirements provided by the user
2. For EACH requirement, evaluate against these 5 criteria:
   - **Clear**: Is the requirement unambiguous?
   - **Testable**: Can compliance be objectively verified?
   - **Traceable**: Does it reference a standard or parent requirement?
   - **Feasible**: Is it technically achievable?
   - **Complete**: Does it include acceptance criteria?
3. Generate the checklist using the Output Format below

## Output Format
Use this EXACT format for each requirement:

| Criteria | Status | Notes |
|----------|--------|-------|
| Clear | PASS/FAIL | [explanation] |
| Testable | PASS/FAIL | [explanation] |
| Traceable | PASS/FAIL | [explanation] |
| Feasible | PASS/FAIL | [explanation] |
| Complete | PASS/FAIL | [explanation] |

**Overall Verdict**: PASS / NEEDS REVISION

## Rules
- NEVER approve a requirement that uses vague language
  ("adequate", "appropriate", "sufficient", "good")
- ALWAYS flag missing regulatory references
- ALWAYS suggest a revised requirement text if verdict is
  NEEDS REVISION
- Output must be in Markdown format
```

#### 3. Test your skill

1. Open the Antigravity CLI from your project folder:
   ```powershell
   agy
   ```
2. The CLI will automatically detect the new skill. Test it:
   ```
   Review the following requirements using the design review checklist:

   1. "The device should be safe."
   2. "The device shall operate continuously for a minimum of 4 hours 
      on a fully charged battery, as measured per IEC 62133."
   3. "The software shall respond to user input within an appropriate 
      time frame."
   ```
3. The AI should follow your skill's procedure and produce a structured checklist for each requirement, correctly flagging #1 and #3 as needing revision.

---

### The "SOP-to-Skill" Mental Model

Every skill you create follows the same pattern as a Standard Operating Procedure:

| SOP Element | Skill Equivalent |
|-------------|------------------|
| Title & Scope | YAML `name` and `description` |
| Purpose | `## Purpose` section |
| Procedure | `## Procedure` section (numbered steps) |
| Acceptance Criteria | `## Output Format` (exact template) |
| Constraints | `## Rules` (guardrails and prohibitions) |

If you can write an SOP, you can write a skill.

---

## ✅ Verification Checkpoint

| Check | How to Verify | Expected Result |
|-------|--------------|----------------|
| Skill directory exists | `ls .agent/skills/design-review-checklist/` | `SKILL.md` present |
| YAML frontmatter valid | Open `SKILL.md`, check `---` markers | Name and description present |
| Skill detected by CLI | Start `agy`, ask about available skills | Your skill is listed |
| Output follows format | Run the test prompt | Structured checklist with PASS/FAIL |
| Rules enforced | Submit a vague requirement | AI flags it as NEEDS REVISION |

---

## Next Steps

- Explore and adapt existing skills in public repositories like the [Anthropic Skills Repository](https://github.com/anthropics/skills) and [Awesome Copilot Skills](https://github.com/awesome-copilot)
- Create additional skills for your team's workflows (e.g., CAPA review, risk assessment)
- Move proven skills from project-level to user-level for use across all projects
- Test in a non-production environment before deploying to Enterprise level

You are now ready for **Module: MCP Servers & Tool Integration**.
