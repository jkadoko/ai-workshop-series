# Governance, Audit Trails & Human-in-the-Loop

## Overview

You now have the tools to build powerful AI-assisted workflows: Python environments, version control, structured prompts, skills, and MCP-connected tools. But in a regulated industry, **having the capability is not enough**—you must also prove that you used it responsibly.

This module addresses the most critical question regulators will ask:

> **"How do you ensure that AI-generated outputs are reviewed, verified, and approved by qualified humans before they are used in production?"**

Think of this as the difference between a robot assembling a medical device and a robot assembling a medical device **under the supervision of a qualified technician who inspects every unit before release**. The robot does the work; the human ensures the quality.

> **Why This Matters:**
> On April 2, 2026, the FDA issued [Warning Letter 320-26-58](https://www.fda.gov/inspections-compliance-enforcement-and-criminal-investigations/warning-letters/purolea-cosmetics-lab-722591-04022026) to **Purolea Cosmetics Lab** (Livonia, MI) citing **improper reliance on AI agents for cGMP documentation** without adequate human oversight. The company used AI to generate drug product specifications, SOPs, and master production records — and when asked why they hadn't performed required process validation, they stated the AI agent "never informed them" it was legally required. This is the first FDA warning letter to explicitly cite AI misuse, and it signals that regulators are watching.

---

## Key Concepts

### Human-in-the-Loop (HITL)

**Human-in-the-Loop** means that a qualified person reviews and approves AI-generated outputs before they are used in any regulated process. This is not optional—it is a regulatory expectation.

```
┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
│  AI      │ ──► │  Draft   │ ──► │  Human   │ ──► │ Approved │
│  Agent   │     │  Output  │     │  Review  │     │  Output  │
│  (auto)  │     │  (draft) │     │ (verify) │     │ (final)  │
└──────────┘     └──────────┘     └──────────┘     └──────────┘
```

**The Three Levels of HITL:**

| Level | Description | When to Use |
|-------|------------|------------|
| **Human-in-the-Loop** | Human reviews and approves every output | Safety-critical decisions, regulatory submissions |
| **Human-on-the-Loop** | Human monitors and can intervene | Batch processing, routine reports |
| **Human-in-Command** | Human defines goals and constraints; AI executes within bounds | Automated testing, data processing pipelines |

> **For regulated industries, the minimum standard is "Human-in-the-Loop" for any output that becomes part of the Design History File (DHF), quality record, or regulatory submission.**

---

### Audit Trails

An **audit trail** is a chronological record of who did what, when, and why. For AI-assisted workflows, the audit trail must capture:

| Element | What to Record | Example |
|---------|---------------|---------|
| **Identity** | Who initiated the workflow | "Engineer: Jane Smith, Badge #1234" |
| **Prompt** | What instruction was given to the AI | The full text of the prompt used |
| **Context** | What data/documents the AI accessed | "Referenced: SRS-2024-001 Rev C" |
| **Output** | What the AI produced | The full generated document/analysis |
| **Review** | Who reviewed and what they decided | "Reviewed by: John Doe, PASS, 2026-06-06" |
| **Modifications** | What the reviewer changed | "Revised Section 3.2 acceptance criteria" |
| **Approval** | Final sign-off | "Approved by: Quality Manager, 2026-06-06" |

---

### Continuous Software Assurance (CSA)

Traditional software validation uses a **point-in-time** approach: validate once, document it, and move on. But AI systems evolve—models update, training data changes, and tool capabilities expand.

**Continuous Software Assurance (CSA)** replaces this with an ongoing validation model:

| Traditional CSV | Continuous Assurance (CSA) |
|----------------|---------------------------|
| Validate once at deployment | Validate continuously throughout lifecycle |
| Fixed test scripts | Risk-based testing that adapts to changes |
| Heavy upfront documentation | Living documentation that evolves |
| Change = full revalidation | Change = targeted risk assessment |

> **Key Insight:** When you update a skill file (SKILL.md), add a new MCP server, or change a prompt template, you are making a change to a validated system. CSA means you assess the risk of each change and test accordingly—not revalidate everything from scratch.

---

## Designing HITL Workflows

### The Review-Approve-Release Pattern

For any AI-generated output in a regulated context, follow this pattern:

```
1. GENERATE  — AI produces a draft output
2. REVIEW    — Qualified person examines the output
3. VERIFY    — Reviewer checks against source documents
4. APPROVE   — Reviewer signs off (or rejects with reasons)
5. RELEASE   — Approved output enters the official record
```

### Practical Implementation

In your agentic workflows, implement HITL checkpoints by:

1. **Saving AI outputs as draft files** — never directly overwriting production documents
2. **Adding review metadata** — include a review block at the top of every AI-generated document:
   ```markdown
   ---
   generated_by: Antigravity CLI + design-review-checklist skill
   generated_date: 2026-06-06
   reviewed_by: [PENDING]
   review_date: [PENDING]
   review_status: DRAFT - NOT APPROVED
   ---
   ```
3. **Using Git for approval tracking** — the reviewer's commit serves as the approval record:
   ```powershell
   git add design_review_report.md
   git commit -m "APPROVED: Design review report for REQ-001 through REQ-010. Reviewed by J. Smith."
   ```

---

## Hands-On Exercise: HITL Design Verification Workflow

### Scenario

You are a design verification engineer. You need to generate test protocols for 3 medical device requirements using the AI, then review and approve them.

### Step 1: Generate Draft Test Protocols

1. Open the Antigravity CLI (launch `agy` in PowerShell)
2. Use this structured prompt:
   ```
   You are a verification test engineer for a Class II medical device.

   Generate test protocol drafts for the following requirements:

   1. "The device shall operate continuously for 4 hours on a fully 
      charged battery per IEC 62133."
   2. "The device enclosure shall achieve an IP rating of IPX4 per 
      IEC 60529."
   3. "The device shall not produce electromagnetic emissions exceeding 
      limits specified in IEC 60601-1-2."

   For each requirement, provide:
   - **Test ID**: Format TV-001, TV-002, etc.
   - **Requirement Traced**: [exact requirement text]
   - **Test Method**: [numbered step-by-step procedure]
   - **Acceptance Criteria**: [specific, measurable]
   - **Equipment Required**: [list with calibration requirements]
   - **Regulatory Reference**: [standard and section]
   - **Review Status**: DRAFT - PENDING REVIEW
   ```

3. Copy the output and save it to a file:
   ```powershell
   # In VS Code or Notepad, save as:
   # test_protocols_draft.md
   ```

### Step 2: Review the Output

Examine each generated test protocol as a qualified engineer:

1. ✅ Is the test method technically sound?
2. ✅ Are the acceptance criteria specific and measurable?
3. ✅ Is the correct regulatory standard referenced?
4. ✅ Are all required equipment items listed?
5. ❓ Are there any gaps, hallucinations, or incorrect references?

> **Common AI Issues to Watch For:**
> - Citing standards that don't exist or incorrect section numbers
> - Acceptance criteria that are vague ("shall perform adequately")
> - Missing critical test steps
> - Equipment specifications that are unrealistic

### Step 3: Document Your Review

1. Add a review header to the file:
   ```markdown
   ---
   generated_by: Antigravity CLI
   generated_date: 2026-06-06
   reviewed_by: [Your Name]
   review_date: 2026-06-06
   review_status: APPROVED WITH MODIFICATIONS
   modifications: "Corrected IEC 60601-1-2 section reference in TV-003"
   ---
   ```

2. Make any necessary corrections directly in the document

### Step 4: Commit as the Approval Record

1. Stage and commit your reviewed document:
   ```powershell
   git add test_protocols_draft.md
   git commit -m "APPROVED: Test protocols TV-001 through TV-003. Reviewed by [Your Name]. Modified TV-003 regulatory reference."
   ```

2. View the audit trail:
   ```powershell
   git log --oneline
   ```

You now have a complete, traceable record: the AI generated the draft, you reviewed it, documented your findings, and committed the approved version with a descriptive message.

---

## Regulatory Landscape (2026)

### FDA

| Guidance | Status | Key Requirement |
|----------|--------|----------------|
| PCCP (Predetermined Change Control Plans) | Final (Aug 2025) | Pre-authorize certain AI model updates within original submission |
| Total Product Lifecycle (TPLC) | Active enforcement | AI compliance is continuous, not point-in-time |
| GMLP (Good Machine Learning Practices) | Draft guidance | Comprehensive practices for AI/ML medical devices |
| April 2026 Warning Letter | Enforcement precedent | Cited improper AI reliance without human oversight |

### EU AI Act

| Provision | Timeline | Impact |
|-----------|----------|--------|
| High-risk AI classification | Enforceable 2026 | Medical device AI requires conformity assessment |
| Transparency requirements | Enforceable 2026 | Users must be informed when interacting with AI |
| Risk management | Enforceable 2026 | Must implement and document AI risk controls |

---

## ✅ Verification Checkpoint

| Check | How to Verify | Expected Result |
|-------|--------------|----------------|
| HITL workflow completed | Generated, reviewed, and committed test protocols | File in Git with approval commit |
| Review metadata present | Check file header | Contains reviewer name, date, status |
| Audit trail exists | `git log` | Commit message documents approval |
| Modifications documented | Check file and commit message | Any changes are described |

You are now ready for **Module: Capstone — Automated Design Verification**.
