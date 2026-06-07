# Installing Python

## Overview

Welcome to the "operating room." Now that you are comfortable with the terminal, it is time to install Python—one of the most powerful and widely used programming languages in the world.

In the medical device field, Python is incredibly useful for:
- Automating data analysis
- Processing large datasets from clinical trials
- Testing hardware logic

> **Why Python for Regulated Industries?**
> Python is the dominant language in data science, automation, and AI—all areas where regulated industries are rapidly adopting new workflows. Unlike proprietary tools (e.g., MATLAB, LabVIEW), Python is open-source, meaning your processes are transparent and auditable. The FDA's own Center for Devices and Radiological Health (CDRH) uses Python-based tools in its regulatory science programs. Learning Python is not just a technical skill—it is a strategic investment in your career.

We will install Python using the Microsoft Store. This is the safest, most reliable method for Windows users because it automatically configures your system settings in the background.

---

## Installation Steps

### 1. Open the Microsoft Store

**Justification:** The Microsoft Store provides an official, verified, and automatically updating version of Python. Think of this like using an FDA-cleared software package directly from the manufacturer.

**Steps:**
1. Click your Windows Start menu
2. Type "Microsoft Store"
3. Press Enter to open the application

---

### 2. Search for and Install Python

**Justification:** Python comes in multiple versions. Grabbing the latest stable release ensures you have the most up-to-date tools and security features without the bugs of experimental builds.

**Steps:**
1. In the search bar at the top of the Microsoft Store, type "Python"
2. Select the most recent version (e.g., Python 3.13 or 3.14) published by the Python Software Foundation
3. Click the **Get** or **Install** button
4. Wait a few moments for the download and installation to complete

---

### 3. Open Windows PowerShell

**Justification:** Just like the Antigravity CLI exercise, we need a command-line interface to interact with Python. Python does not have a traditional graphical user interface with buttons; it receives commands through the terminal.

**Steps:**
1. Click the Windows Start menu
2. Type "PowerShell"
3. Select "Windows PowerShell"

---

### 4. Verify the Installation

**Justification:** In quality assurance, you always verify that equipment is calibrated before use. We need to ensure Python is installed and properly recognized by your computer's system.

**Steps:**
1. In your PowerShell window, type:
   ```powershell
   python --version
   ```
2. Press Enter
3. If successful, you should see the version number (e.g., `Python 3.14.2`)

> **Note:** If it opens the Microsoft Store again, close PowerShell, wait a minute for the background installation to finish, and try again.

---

### 5. Write Your First Script in Notepad

**Justification:** A Python "script" is simply a plain text file containing instructions. We use a basic editor like Notepad because word processors like Microsoft Word add hidden formatting that can break code.

**Steps:**
1. Open the Windows Start menu
2. Type "Notepad"
3. Press Enter
4. In the blank document, type the following line exactly:
   ```python
   print("Design verification complete. Python is running!")
   ```

This `print` command tells Python to display the text inside the quotation marks.

---

### 6. Save Your Python File

**Justification:** The file must be saved with a `.py` extension so the operating system knows it contains Python code, not just regular text.

**Steps:**
1. In Notepad, click **File > Save As**
2. Choose a location that is easy to find, like your **Desktop**
3. In the "File name" box, type: `test_script.py`
4. In the "Save as type" dropdown, select **All Files** 
   > ⚠️ This is critical—it prevents Notepad from accidentally saving it as a `.txt` text file
5. Click **Save**

---

### 7. Execute the Script (Drag-and-Drop Method)

**Justification:** Now it is time to execute your written procedure. The terminal needs to know exactly where your file lives on the hard drive. Typing out file paths can be error-prone, so we will use a drag-and-drop shortcut.

**Steps:**
1. Go back to your PowerShell window
2. Type `python ` (include a single space after "python", but do NOT press Enter yet)
3. Find your `test_script.py` file on your Desktop
4. Click and drag the file icon directly into the PowerShell window and release
5. PowerShell will automatically type out the exact file path for you
6. Press Enter

---

## Success!

You should immediately see your message:

```
Design verification complete. Python is running!
```

printed directly in the terminal. You have successfully written and executed your first piece of software!

---

## ✅ Verification Checkpoint

Before moving on, confirm the following:

| Check | How to Verify | Expected Result |
|-------|--------------|----------------|
| Python installed | `python --version` | Version number (e.g., `Python 3.14.2`) |
| Script executed | Run `test_script.py` | Message printed in terminal |
| Path configured | Python runs from any directory | No "command not found" errors |

If all checks pass, you are ready for **Module 2: Python Package Management (pip)**.
