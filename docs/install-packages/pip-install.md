# Installing Python Packages with pip

## Overview

Welcome to the next level of programming!

Out of the box, Python is incredibly capable, but its true power comes from its massive ecosystem of external **packages** (also called libraries).

Think of base Python as a standard, high-quality power unit in a hospital. External packages are the specialized attachments you plug into it—like an ultrasound wand, an ECG monitor, or a surgical drill.

**Pandas** is one of these "attachments." It is the industry standard for data analysis, allowing you to manipulate massive spreadsheets and clinical data sets with just a few lines of code.

To download these attachments, Python uses a built-in tool called **pip** (which stands for "Pip Installs Packages"). Think of pip as your automated supply chain or procurement system.

---

## Installation Steps

### 1. Open Windows PowerShell

**Justification:** Just like before, we need our command-line "operating room" to send instructions to the computer.

**Steps:**
1. Click your Windows **Start menu**
2. Type "PowerShell"
3. Press **Enter**

---

### 2. Create a Virtual Environment (Clean Room)

**Justification:** In a hospital, you would never mix instruments between operating rooms. Similarly, in software development—especially in regulated industries—we isolate each project's dependencies in a **virtual environment**. This ensures that installing a package for one project does not interfere with another, and it creates a reproducible, auditable record of exactly which tools were used.

**Steps:**
1. Navigate to your project folder. For example, if your project is on the Desktop:
   ```powershell
   cd ~/Desktop
   mkdir my-first-project
   cd my-first-project
   ```
2. Create a virtual environment named `.venv`:
   ```powershell
   python -m venv .venv
   ```
3. Activate the virtual environment:
   ```powershell
   .venv\Scripts\Activate
   ```
4. You should see `(.venv)` appear at the beginning of your command prompt. This confirms you are now working inside the isolated environment.

> **Note:** Every time you open a new PowerShell window to work on this project, you must re-activate the virtual environment by running `.venv\Scripts\Activate` from the project folder.

---

### 3. Verify Your Supply Chain (pip)

**Justification:** In standard operating procedures, we always verify our tools are present before starting a procedure. Because you installed Python from the Microsoft Store, pip was automatically installed alongside it.

**Steps:**
1. In PowerShell (with your virtual environment active), type:
   ```powershell
   pip --version
   ```
2. Press **Enter**
3. You should see a response showing the version number (e.g., `pip 24.0`). This confirms your procurement tool is ready to go.

---

### 4. Install the Pandas Package

**Justification:** We need to command pip to reach out to the official Python repository on the internet, download the pandas tool, and install it into your virtual environment.

**Steps:**
1. In PowerShell, type exactly:
   ```powershell
   pip install pandas
   ```
2. Press **Enter**
3. You will see several progress bars appear as pip downloads pandas and a few other dependent files it needs to function. Once it returns to a blinking cursor, the installation is complete.

---

### 5. Record Your Dependencies

**Justification:** In regulated industries, every tool used in a process must be documented. The `requirements.txt` file serves as your "Bill of Materials"—an exact record of every package and version installed in your environment. This is critical for reproducibility and audits.

**Steps:**
1. In PowerShell, type:
   ```powershell
   pip freeze > requirements.txt
   ```
2. Press **Enter**
3. This creates a file called `requirements.txt` in your project folder. Open it in Notepad to see the exact packages and versions that were installed.

> **Tip:** If a colleague needs to replicate your environment, they can run `pip install -r requirements.txt` inside their own virtual environment to get the exact same setup.

---

### 6. Write a Verification Script

**Justification:** We must perform an Installation Qualification (IQ) to prove the software was installed correctly and can be accessed by Python.

**Steps:**
1. Open **Notepad** (Start menu → type "Notepad")
2. Type the following two lines exactly as shown:
   ```python
   import pandas as pd
   print("Pandas successfully imported and ready for data analysis!")
   ```
   > *Note: `import pandas` is the command that tells Python to "plug in the attachment" before running the rest of the script.*

---

### 7. Save the Script

**Justification:** We need to save this as a Python file so we can execute it.

**Steps:**
1. Click **File** > **Save As**
2. Save it to your project folder (`my-first-project`)
3. In the "File name" box, type: `test_pandas.py`
4. In the "Save as type" dropdown, select **All Files**
   > ⚠️ This is critical—it prevents Notepad from accidentally saving it as a `.txt` text file
5. Click **Save**

---

### 8. Execute the Script

**Justification:** Running the script proves that Python can successfully locate and utilize the new pandas package you installed inside your virtual environment.

**Steps:**
1. Go back to your PowerShell window (make sure you still see `(.venv)` in the prompt)
2. Type `python ` (include a single space after "python", but do NOT press Enter yet)
3. Find your `test_pandas.py` file in your project folder
4. Click and drag the file icon directly into the PowerShell window and release
5. PowerShell will automatically type out the exact file path for you
6. Press **Enter**

---

## Success!

If you see the message **"Pandas successfully imported and ready for data analysis!"** printed on your screen, congratulations! You have successfully:

1. ✅ Created an isolated virtual environment
2. ✅ Installed a third-party package using pip
3. ✅ Documented your dependencies in `requirements.txt`
4. ✅ Verified the installation with a test script

You are now fully equipped to start automating complex spreadsheet tasks with a reproducible, audit-ready setup.