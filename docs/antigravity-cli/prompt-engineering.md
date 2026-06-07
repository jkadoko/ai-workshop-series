# Prompt Engineering for Compliance

## Overview

You have installed the Antigravity CLI and can ask it questions. But in regulated industries, "asking questions" is not enough. The AI's output must be **structured, reproducible, and audit-ready**.

Think of the difference between asking a colleague "What do you think about the design?" versus handing them a formal Design Review Checklist with specific criteria, pass/fail fields, and a signature block. The first approach produces inconsistent, unreliable results. The second produces documentation you can submit to the FDA.

**Prompt engineering** is the discipline of crafting precise instructions that transform an AI from an unpredictable conversationalist into a reliable, repeatable tool.

> **Why This Matters in Regulated Industries:**
> The FDA's April 2026 warning letter cited a manufacturer for "improper reliance on AI agents" without adequate human oversight. One contributing factor was unstructured prompts that produced inconsistent outputs across different runs. Structured prompting is not just a best practice—it is becoming a compliance expectation.

---

## Concepts

### Chat vs. Structured Prompting

| Approach | Example | Output Quality |
|----------|---------|---------------|
| **Chat** (unstructured) | "Tell me about biocompatibility testing" | Variable, conversational, hard to audit |
| **Structured** | "List the 5 ISO 10993 biocompatibility test categories with a one-line description for each. Format as a Markdown table." | Consistent, formatted, reproducible |

### The Anatomy of a Reliable Prompt

A well-engineered prompt for regulated work has six essential components:

```
┌──────────────────────────────────────────────┐
│  1. PERSONA (ROLE) — Who is the AI?          │
│  2. CONTEXT        — Background information  │
│  3. TASK           — Specific actions        │
│  4. EXAMPLES       — Reference input/outputs │
│  5. FORMAT         — Structure of output     │
│  6. SELF-REVIEW    — Verification checks     │
└──────────────────────────────────────────────┘
```

**Example:**
```
You are a quality assurance engineer reviewing medical device design requirements.

Context: The device is a Class II battery-powered surgical instrument regulated under FDA 21 CFR 878.

Task: Review the requirement provided under Input and identify any gaps in testability, clarity, or regulatory alignment.

Examples:
- Input: "The device should work well."
- Output: 
  - Requirement: "The device should work well."
  - Assessment: Needs Revision
  - Gaps Identified: Uses subjective term ("well"); lacks objective metrics.
  - Suggested Revision: "The device shall meet the durability requirements of IEC 60601-1."
  - Regulatory Reference: IEC 60601-1

Format: Respond using this exact structure for the Input requirement:
- **Requirement**: [restate the requirement]
- **Assessment**: [Pass / Needs Revision]
- **Gaps Identified**: [bulleted list]
- **Suggested Revision**: [improved requirement text]
- **Regulatory Reference**: [applicable standard or regulation]

Self-review: Before providing your output, check it against the following constraints:
1. If the requirement uses subjective terms (like "good", "fast", "safe"), the assessment must be "Needs Revision".
2. You must cite a relevant standard or regulatory section.
```

---

## Techniques

### 1. Output Format Constraints

Always specify the exact output format you need. This eliminates variability and makes outputs directly usable in documentation.

**Common formats for regulated work:**

| Format | When to Use | Example Instruction |
|--------|------------|-------------------|
| **Markdown table** | Comparison, summaries | "Format as a Markdown table with columns: Requirement, Status, Evidence" |
| **JSON** | Machine-readable data | "Return results as a JSON array with keys: id, requirement, verdict, rationale" |
| **Numbered list** | Sequential procedures | "List the steps as a numbered procedure" |
| **Pass/Fail** | Binary assessments | "For each item, state PASS or FAIL with a one-sentence justification" |

---

### 2. The "Explain Before You Execute" Pattern

In regulated industries, decisions must be justified. Train the AI to show its reasoning before producing a final answer.

**Prompt template:**
```
Before providing your final answer, explain your reasoning step by step.
Cite the specific standards, regulations, or evidence that support each step.
Then provide your final answer in the format specified below.
```

**Why this works:** It creates a built-in audit trail within the output itself. If a reviewer questions the AI's conclusion, the reasoning chain is already documented.

---

### 3. Context Boundaries (PII/Proprietary Data Protection)

When working with AI in regulated industries, you must control what information you share.

**Rules:**
- ❌ Never paste patient data, PII, or PHI into an AI prompt
- ❌ Never share proprietary trade secrets or unpublished IP
- ✅ Use anonymized or synthetic data
- ✅ Describe the *shape* of the problem without revealing sensitive details

**Example — BAD:**
```
Review this patient record: John Smith, DOB 03/15/1980, MRN 12345...
```

**Example — GOOD:**
```
I have a dataset of patient demographics with fields: age (integer),
gender (M/F/Other), diagnosis_code (ICD-10 string), and device_serial
(alphanumeric). Write a Python script to validate that all records
have non-null values for required fields.
```

---

### 4. Reproducibility Through Explicit Constraints

For audit purposes, your prompts should produce the same output regardless of who runs them or when.

**Techniques:**
- Fix the output length: "Provide exactly 5 items"
- Restrict the scope: "Only consider requirements in Section 3.2"
- Eliminate opinion: "Base your assessment only on ISO 14971:2019 Section 4"
- Lock the format: "Use this exact template for each item"

---

## Hands-On Exercises

### Exercise 1: Unstructured vs. Structured Comparison

**Goal:** Experience the difference between unstructured and structured prompting.

1. Open your Antigravity CLI by launching `agy` in PowerShell.
2. First, try the unstructured prompt:
   ```
   Tell me about risk management for medical devices.
   ```
3. Read the response. Note how it is general, conversational, and would be difficult to use in a formal document.

4. Now try the structured prompt:
   ```
   You are a risk management specialist for Class II medical devices.

   Task: List the 5 key activities required by ISO 14971:2019 for risk management.

   Format: Respond as a Markdown table with columns:
   - Activity Number
   - Activity Name
   - Purpose (one sentence)
   - Key Deliverable
   ```
5. Compare the two outputs. The structured version is directly usable in a training document or SOP.

---

### Exercise 2: Design Requirement Review

**Goal:** Use the AI as an automated design reviewer.

1. In your interactive `agy` CLI shell, paste this prompt:
   ```
   You are a systems engineer reviewing design inputs for a battery-powered
   surgical instrument (FDA Class II, 21 CFR 878).

   Review each requirement below and assess its compliance with good
   requirements practices (clear, testable, unambiguous, traceable).

   Requirements:
   1. "The device should work well."
   2. "The device shall operate continuously for a minimum of 4 hours
      on a fully charged battery, as measured per IEC 62133."
   3. "Battery life must be good enough for surgery."

   Format for EACH requirement:
   - **Requirement**: [exact text]
   - **Verdict**: PASS or FAIL
   - **Issues**: [bulleted list, or "None"]
   - **Suggested Revision**: [improved text, or "N/A"]
   ```

2. Review the output. Notice how the AI correctly identifies vague requirements (#1, #3) and approves the well-written one (#2).

---

### Exercise 3: Audit-Ready Output Generation

**Goal:** Generate output that could be included directly in a compliance document.

1. Run the query directly from your PowerShell command line using `agy query`:
   ```powershell
   agy query 'You are documenting a design verification test for a medical device. Generate a test protocol entry for the requirement: "The device shall withstand a drop from 1 meter onto a hard surface without loss of function, per IEC 60601-1 Section 15.3.4." Format the output as follows: - **Test ID**: [auto-generate, format TV-###] - **Requirement Traced**: [restate the requirement] - **Test Method**: [step-by-step procedure] - **Acceptance Criteria**: [specific, measurable criteria] - **Equipment Required**: [list] - **Regulatory Reference**: [standard and section]'
   ```

---

## Anti-Patterns to Avoid

| Anti-Pattern | Why It Fails | Better Approach |
|-------------|-------------|----------------|
| "Make it good" | Subjective, not measurable | Specify exact criteria |
| "Be thorough" | Undefined scope | "Address items 1-5 from Section 3" |
| No format specified | Output varies every run | Provide an exact template |
| Pasting real patient data | HIPAA violation | Use synthetic/anonymized data |
| Trusting output without review | Over-reliance on AI | Always verify against source documents |

---

## ✅ Verification Checkpoint

| Check | How to Verify | Expected Result |
|-------|--------------|----------------|
| Structured prompt produces table | Run Exercise 1 structured prompt in `agy` | Markdown table output |
| Requirement review works | Run Exercise 2 | PASS/FAIL for each requirement |
| Output is audit-ready | Run Exercise 3 | Formatted test protocol entry |
| No PII in prompts | Review your prompt history | No real names, MRNs, or DOBs |

You are now ready for **Module: Agentic Workflows & Skills**.
