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
assesses each requirement against standard quality criteria used
in medical device design controls (FDA 21 CFR 820, ISO 13485).

## Procedure
1. Read the list of requirements provided by the user
2. For EACH requirement, evaluate against these 5 criteria:
   - **Clear**: Is the requirement unambiguous? Does it use precise, measurable language?
   - **Testable**: Can compliance be objectively verified through testing, analysis, or inspection?
   - **Traceable**: Does it reference a parent requirement, standard, or regulatory source?
   - **Feasible**: Is it technically achievable with current technology and resources?
   - **Complete**: Does it include acceptance criteria, units of measure, and boundary conditions?
3. Generate the checklist using the Output Format below
4. Provide an overall summary with statistics

## Output Format

For each requirement, use this EXACT format:

### Requirement [N]: "[requirement text]"

| Criteria | Status | Notes |
|----------|--------|-------|
| Clear | PASS/FAIL | [explanation] |
| Testable | PASS/FAIL | [explanation] |
| Traceable | PASS/FAIL | [explanation] |
| Feasible | PASS/FAIL | [explanation] |
| Complete | PASS/FAIL | [explanation] |

**Overall Verdict**: PASS / NEEDS REVISION

**Suggested Revision** (if NEEDS REVISION): [improved requirement text]

---

After all requirements are reviewed, provide a summary:

### Summary
- Total requirements reviewed: [N]
- PASS: [N]
- NEEDS REVISION: [N]
- Most common issue: [description]

## Rules
- NEVER approve a requirement that uses vague language ("adequate", "appropriate", "sufficient", "good", "reasonable", "acceptable")
- ALWAYS flag missing regulatory references as a FAIL for Traceable
- ALWAYS flag missing units of measure or boundary conditions as a FAIL for Complete
- ALWAYS suggest a revised requirement text if verdict is NEEDS REVISION
- The suggested revision MUST use "shall" (mandatory) language, not "should" or "may"
- Output must be in Markdown format
- Do NOT add requirements that were not provided by the user
- Do NOT change the meaning of requirements when suggesting revisions
