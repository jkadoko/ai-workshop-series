# Module 2.6: MS Office Document Automation (Word & Excel)

## Overview

In highly regulated industries (such as medical devices, biotechnology, and pharmaceuticals), standard deliverables are almost always in Microsoft Office formats:
- **Microsoft Word (`.docx`)** for Design Verification Protocols, Standard Operating Procedures (SOPs), Clinical Trial Reports, and regulatory submissions.
- **Microsoft Excel (`.xlsx`)** for design input requirements matrices, risk hazard analyses (FMEA), project plans, and statistical verification logs.

These documents are rarely plain text—they contain complex tables, embedded diagrams, pictures, exact corporate styling, and active calculation formulas. 

This module covers how **AI agents** programmatically manipulate and verify MS Office documents safely, auditably, and with high precision.

---

## The Core Limitation: Binary vs. Structured Data

AI models are text-based. They cannot "open" a binary `.docx` or `.xlsx` file and type into it like a human with a mouse and keyboard. To allow an agent to manipulate these documents, we must bridge the gap using structured formats:

```
┌─────────────────┐      Read (Convert)      ┌────────────────────────┐
│  Binary Office  │ ───────────────────────► │  Structured Text / DOM │
│  Doc (.docx)    │ ◄─────────────────────── │ (Markdown / XML / JSON)│
└─────────────────┘      Write (Compile)     └────────────────────────┘
```

---

## 📄 Word Document (.docx) Workflows

A `.docx` file is not a single file—it is a compressed ZIP archive containing a folder structure of XML text files, styles, and embedded media assets.

### 1. Reading & Analyzing Content (Pandoc Extraction)
To read the text inside a Word document while preserving headings, lists, tables, and tracked changes, we use **Pandoc** to convert the file into Markdown:
```powershell
# Convert a Word document to Markdown including all tracked changes
pandoc --track-changes=all input-document.docx -o output.md
```
*Options for `--track-changes`:*
- `all` — Shows insertions and deletions inline in the Markdown output (perfect for auditing).
- `accept` / `reject` — Simulates accepting or rejecting all tracked changes before extraction.

### 2. Editing & Unpacking (The OOXML Workflow)
When you need to perform precise, structural edits (like editing tables, inserting pictures, or adding comments) while preserving the original styling, the agent uses an **OOXML Unpacking** workflow:

1. **Unpack the Document:** Extract the ZIP contents to a temporary directory:
   ```powershell
   python ooxml/scripts/unpack.py document.docx ./unpacked_doc/
   ```
2. **Access the XML DOM:** The main text and layout live in `./unpacked_doc/word/document.xml`. Media files live in `./unpacked_doc/word/media/`.
3. **Execute Python Edits:** Run a Python script that parses the XML using a secure parser (like `defusedxml`), locates specific nodes (e.g. paragraph tags `<w:p>`, run tags `<w:r>`), modifies them, and saves the XML.
4. **Pack the Document:** Compress the directory back into a valid `.docx` file:
   ```powershell
   python ooxml/scripts/pack.py ./unpacked_doc/ modified_document.docx
   ```

### 3. Redlining (Tracked Changes) Workflow
For compliance documents, changes must be proposed as "Redlines" (tracked changes) rather than direct overwrites. 
To do this programmatically, agents follow the **RSID Run Preserving Principle**:
- Do not replace entire paragraphs (which destroys formatting tags).
- Only wrap modified text runs in `<w:del>` (deletions) and `<w:ins>` (insertions) tags.
- Reuse the original run's RSID (Revision Save ID) to maintain document history compatibility.

*Good OOXML Pattern:*
```xml
<w:r w:rsidR="00AB12CD"><w:t>The term is </w:t></w:r>
<w:del><w:r><w:delText>30</w:delText></w:r></w:del>
<w:ins><w:r><w:t>60</w:t></w:r></w:ins>
<w:r w:rsidR="00AB12CD"><w:t> days.</w:t></w:r>
```

### 4. Word Document Templating & JSON Population
For generating standardized technical reports or procedures, instead of editing complex XML elements directly, a cleaner and more industry-standard technique is to create a `.docx` template with placeholders (e.g. `{{DEVICE_NAME}}`) and programmatically merge them with structured data from a JSON file.
- For a detailed walkthrough of this technique, refer to **[Word Document Templating & JSON Population](docx-templates.md)**.

### 5. Visual Layout Verification
After generating or modifying a document, we must verify that tables are aligned and pictures do not overlap. The agent does this by converting the document to PDF and then to images:
```powershell
# 1. Convert Word to PDF using headless LibreOffice
soffice --headless --convert-to pdf document.docx

# 2. Convert PDF pages to JPEG images for visual inspection
pdftoppm -jpeg -r 150 document.pdf page_preview
```

---

## 📊 Excel Spreadsheet (.xlsx) Workflows

Like Word files, Excel files are XML ZIPs. However, spreadsheets are centered on data structure, formulas, and visual logic.

### 1. Library Selection: Pandas vs. Openpyxl
- **Pandas:** Used for reading, analyzing, sorting, filtering, and performing statistical operations on data sheets. It treats the sheet like a data table (DataFrame).
  ```python
  import pandas as pd
  df = pd.read_excel("raw_data.xlsx", sheet_name="Measurements")
  summary = df.describe()
  ```
- **Openpyxl:** Used when you need to write formulas, style cells, change column widths, manage multiple tabs, or preserve pre-existing spreadsheet formatting.
  ```python
  from openpyxl import load_workbook
  wb = load_workbook("existing.xlsx")
  sheet = wb["Summary"]
  sheet["B5"] = "=SUM(B2:B4)"  # Write Excel formula
  wb.save("existing.xlsx")
  ```

### 2. CRITICAL: Use Formulas, Not Hardcoded Values
In regulated files (such as clinical trial calculations or engineering risk matrices), **always write Excel formulas rather than calculating values in Python and hardcoding the result.** Hardcoding results makes the model static and breaks the audit trail.

- **❌ WRONG (Hardcoding calculated values):**
  ```python
  total = df["Sales"].sum()
  sheet["B10"] = total  # Writes: 5000 (static)
  ```
- **✅ CORRECT (Writing Excel formulas):**
  ```python
  sheet["B10"] = "=SUM(B2:B9)"  # Let Excel calculate it dynamically
  ```

### 3. The Recalculation Requirement (`recalc.py`)
Libraries like `openpyxl` can write formulas as text strings (e.g., `sheet['B10'] = "=SUM(B2:B9)"`), but they **do not compute the values**. If you open the saved file in another script, the cell value is blank.

To ensure your sheet calculations are up-to-date and have no formula errors, you must programmatically trigger a recalculation (which utilizes LibreOffice in the background):
```powershell
python recalc.py output.xlsx
```
The script outputs a JSON report:
- Status: `success` or `errors_found`
- Scans cells for typical errors: `#REF!`, `#DIV/0!`, `#VALUE!`, `#N/A`, `#NAME?`.

---

## 🎨 Spreadsheet Formatting Standards

Unless a pre-existing corporate template takes precedence, adhere to these industry-standard financial and engineering spreadsheet styling rules:

| Element | Style Standard | Hex / RGB Code |
|---------|----------------|----------------|
| **Hardcoded Inputs** | Blue text | `RGB(0, 0, 255)` / `#0000FF` |
| **Calculations / Formulas** | Black text | `RGB(0, 0, 0)` / `#000000` |
| **Worksheet Links** | Green text (pulls from another tab) | `RGB(0, 128, 0)` / `#008000` |
| **External File Links** | Red text (pulls from another file) | `RGB(255, 0, 0)` / `#FF0000` |
| **Assumptions Cell** | Yellow background (warning / attention) | `RGB(255, 255, 0)` / `#FFFF00` |

---

## Hands-On Exercises

### Exercise 1: Extract and Audit a Word Report
**Goal:** Extract the text of a Word document to inspect its changes using Pandoc.

1. Open PowerShell in your project workspace.
2. In the `data/` directory, locate a sample `.docx` document (if none exists, you can run the Capstone preparation step first).
3. Extract the contents to Markdown:
   ```powershell
   pandoc --track-changes=all data/sample_report.docx -o data/extracted_report.md
   ```
4. Open `extracted_report.md` in VS Code. Note how deletions are represented as `~~deleted text~~` and insertions are highlighted, showing the audit history.

---

### Exercise 2: Create a Formatted Excel calculation sheet
**Goal:** Create a reproducible Excel spreadsheet containing dynamic calculations.

1. Create a script named `create_excel.py` in your workspace using VS Code:
   ```python
   from openpyxl import Workbook
   from openpyxl.styles import Font, PatternFill

   wb = Workbook()
   sheet = wb.active
   sheet.title = "Risk Matrix"

   # 1. Add headers
   sheet["A1"] = "Hazard ID"
   sheet["B1"] = "Severity (S)"
   sheet["C1"] = "Probability (P)"
   sheet["D1"] = "Risk Priority Number (RPN)"

   # 2. Add sample hardcoded inputs (styled BLUE)
   blue_font = Font(color="0000FF", bold=True)
   sheet["A2"] = "HZ-001"
   
   sheet["B2"] = 4
   sheet["B2"].font = blue_font
   
   sheet["C2"] = 3
   sheet["C2"].font = blue_font

   # 3. Add calculation formula (styled BLACK)
   sheet["D2"] = "=B2*C2"
   sheet["D2"].font = Font(color="000000", bold=True)

   wb.save("data/hazard_risk_log.xlsx")
   print("Spreadsheet saved successfully.")
   ```
2. Run the script:
   ```powershell
   python create_excel.py
   ```
3. Run the recalculation script to verify there are no formula errors:
   ```powershell
   python recalc.py data/hazard_risk_log.xlsx
   ```
4. Verify the output JSON displays `"total_errors": 0` and `"status": "success"`.

---

## ✅ Verification Checkpoint

| Check | How to Verify | Expected Result |
|-------|--------------|----------------|
| Pandoc installed | Run `pandoc --version` | Version info displayed |
| Excel file generated | Check `data/hazard_risk_log.xlsx` | File exists |
| Calculations dynamic | Open the Excel file in Excel | Cell D2 computes severity * probability |
| Formula validation clean | Run `recalc.py` | JSON output shows status success |
| No hardcoded math | Review Python code | Cells contain formulas (`=B2*C2`), not static integers |

---

## Skill references
For advanced document processing, review the detailed implementation instructions in these standard repositories:
- **DOCX Skill:** [c:\Users\Jonah\.claude\skills\docx\SKILL.md](file:///c:/Users/Jonah/.claude/skills/docx/SKILL.md)
- **XLSX Skill:** [C:\Users\Jonah\.gemini\config\skills\xlsx\SKILL.md](file:///C:/Users/Jonah/.gemini/config/skills/xlsx/SKILL.md)
- **Anthropic Official Skills:** `https://github.com/anthropics/skills.git`
