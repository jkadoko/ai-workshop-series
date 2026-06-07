# Word Document Templating & JSON Population

## Overview

In professional workflows, writing standard reports (such as design verification summaries, audit logs, or test protocols) from scratch every time is highly inefficient. Instead, engineers use **Templates**—pre-formatted documents containing logos, tables, layout styling, and static boilerplate text, with placeholders representing the dynamic information.

An AI agent can read a structured data source (like a **JSON file**) and automatically inject those values into a Word template (`.docx`). This ensures the resulting report is formatted correctly and contains all required data, making validation and QA review straightforward.

---

## 🎨 Designing a Word Template

To create a template, open Microsoft Word and design your document layout. In places where data should be dynamically inserted, type placeholder tags surrounded by double curly braces:

```text
==========================================================
                      TEST REPORT
==========================================================
Project Name: {{PROJECT_NAME}}
Test Engineer: {{ENGINEER_NAME}}
Date: {{TEST_DATE}}

Verification Results:
----------------------------------------------------------
| Requirement ID | Description | Verdict |
----------------------------------------------------------
| REQ-001        | Battery run | {{REQ_001_VERDICT}} |
----------------------------------------------------------
```

---

## ⚠️ The XML "Run Split" Problem

Under the hood, a `.docx` file stores paragraph text as a series of **runs** (represented by `<w:r>` tags in the XML DOM). A run is a contiguous block of text that shares the same formatting (bold, italic, font size).

When you type `{{PROJECT_NAME}}` in Microsoft Word, Word will often split that text across multiple runs in the XML. For example:
- Run 1: `<w:t>{</w:t>`
- Run 2: `<w:t>{PROJECT</w:t>`
- Run 3: `<w:t>_NAME}</w:t>`
- Run 4: `<w:t>}</w:t>`

Because the placeholder is split into separate tags, a simple search-and-replace script looking for the text `{{PROJECT_NAME}}` inside the XML file will fail—the string does not exist contiguously.

### How to Fix the XML Run Split Problem
To ensure placeholders can be programmatically replaced, use one of these strategies:
1. **Clear Formatting:** Highlight the placeholder in Word, right-click, and select "Clear Formatting." This forces Word to collapse the text into a single run.
2. **Re-type in Place:** If you edit a placeholder, delete the entire word and type it cleanly without pausing, copying/pasting, or changing formatting midway.
3. **Run Merging (Recommended):** Write your Python population script to temporarily merge all text runs in a paragraph before searching for placeholders. This makes your automation highly robust.

---

## 📋 JSON Data Mapping

Before running the population script, structure your data in a clean, standard JSON file.

*Example data file (`data/report_data.json`):*
```json
{
  "PROJECT_NAME": "Ventilator Battery Module",
  "ENGINEER_NAME": "Jane Doe, QA Engineer",
  "TEST_DATE": "2026-06-06",
  "REQ_001_VERDICT": "PASS"
}
```

---

## 🐍 Python Template Population Script

The following script uses the `python-docx` library to populate templates. It includes a robust helper function that temporarily merges runs inside each paragraph to bypass the XML run-splitting issue, while preserving basic character formatting.

### 1. Install Dependency
Before running, make sure `python-docx` is installed in your virtual environment:
```powershell
pip install python-docx
```

### 2. The Population Code (`populate_template.py`)
```python
import json
import os
from docx import Document

def replace_placeholders_in_paragraph(paragraph, data):
    """Replaces placeholder tags in a paragraph while preserving basic formatting."""
    # Combine run text to check for placeholders
    full_text = "".join(run.text for run in paragraph.runs)
    
    modified = False
    for key, value in data.items():
        placeholder = f"{{{{{key}}}}}"  # Creates: {{KEY}}
        if placeholder in full_text:
            full_text = full_text.replace(placeholder, str(value))
            modified = True
            
    if modified:
        # If the paragraph was modified, save the formatting of the first run
        if len(paragraph.runs) > 0:
            first_run = paragraph.runs[0]
            bold = first_run.bold
            italic = first_run.italic
            font_size = first_run.font.size if first_run.font else None
            font_name = first_run.font.name if first_run.font else None
            color = first_run.font.color.rgb if (first_run.font and first_run.font.color) else None
            
            # Clear all existing runs in the paragraph
            paragraph.text = ""
            
            # Recreate as a single run with the replaced text
            new_run = paragraph.add_run(full_text)
            new_run.bold = bold
            new_run.italic = italic
            if font_size:
                new_run.font.size = font_size
            if font_name:
                new_run.font.name = font_name
            if color:
                new_run.font.color.rgb = color
        else:
            paragraph.text = full_text

def populate_report(template_path, output_path, json_path):
    # Load data
    with open(json_path, "r", encoding="utf-8") as f:
        data = json.load(f)
        
    # Open template document
    doc = Document(template_path)
    
    # Replace placeholders in standard paragraphs
    for paragraph in doc.paragraphs:
        replace_placeholders_in_paragraph(paragraph, data)
        
    # Replace placeholders in table cells
    for table in doc.tables:
        for row in table.rows:
            for cell in row.cells:
                for paragraph in cell.paragraphs:
                    replace_placeholders_in_paragraph(paragraph, data)
                    
    # Save populated document
    doc.save(output_path)
    print(f"Successfully generated populated report: {output_path}")

if __name__ == "__main__":
    populate_report(
        template_path="data/template_report.docx",
        output_path="data/populated_report.docx",
        json_path="data/report_data.json"
    )
```

---

## Hands-On Exercise

### Step 1: Create a Template Document
1. In VS Code, write a small Python script to generate a basic `.docx` template with placeholders:
   ```python
   # create_template.py
   from docx import Document
   
   doc = Document()
   doc.add_heading("Design Verification Report", level=0)
   doc.add_paragraph("Project Name: {{PROJECT_NAME}}")
   doc.add_paragraph("Lead Engineer: {{ENGINEER_NAME}}")
   doc.add_paragraph("Execution Date: {{TEST_DATE}}")
   
   # Add a table
   table = doc.add_table(rows=2, cols=2)
   hdr_cells = table.rows[0].cells
   hdr_cells[0].text = 'Requirement ID'
   hdr_cells[1].text = 'Verdict'
   
   row_cells = table.rows[1].cells
   row_cells[0].text = 'REQ-001'
   row_cells[1].text = '{{REQ_001_VERDICT}}'
   
   doc.save("data/template_report.docx")
   print("Template created.")
   ```
2. Run the script: `python create_template.py`

### Step 2: Create a JSON Data File
Create the file `data/report_data.json` containing:
```json
{
  "PROJECT_NAME": "Pacemaker Lead Verification",
  "ENGINEER_NAME": "David Miller, Lead QA",
  "TEST_DATE": "2026-06-06",
  "REQ_001_VERDICT": "PASS"
}
```

### Step 3: Run the Population Script
1. Save the template population script code above as `populate_template.py`.
2. Run it in your terminal:
   ```powershell
   python populate_template.py
   ```
3. Open `data/populated_report.docx` in Word or extract it using Pandoc to verify that all placeholders were successfully replaced.

---

## ✅ Verification Checkpoint

| Check | How to Verify | Expected Result |
|-------|--------------|----------------|
| python-docx installed | Run `pip show python-docx` | Package details displayed |
| JSON data valid | Open `data/report_data.json` | JSON parses without syntax errors |
| Replacement works | Populate report and check results | Document contains "David Miller" instead of placeholders |
| Table cells populated | Check table in `populated_report.docx` | Cell displays "PASS" |
