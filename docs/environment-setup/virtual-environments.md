# Python Virtual Environments

## Overview

In a hospital, you would never reuse surgical instruments between patients without sterilization. Each procedure gets its own **sterile field**—a clean, isolated workspace with only the tools required for that specific operation.

Virtual environments work the same way for software. A **virtual environment** is an isolated copy of Python and its packages, dedicated to a single project. This ensures that:

- Installing a package for Project A does not break Project B
- You can reproduce the exact same environment months or years later
- Auditors and regulators can verify exactly which tools were used

> **Why This Matters in Regulated Industries:**
> The FDA's Quality Management System Regulation (QMSR) and IEC 62304 both require that software tools used in product development are documented and controlled. A virtual environment, combined with a `requirements.txt` file, provides the "Bill of Materials" that satisfies this documentation requirement.

---

## Concepts

### What is a Virtual Environment?

When you install Python, it creates a **global** environment—a single shared space where all packages are installed. This is fine for casual use, but dangerous for professional work because:

- Two projects might need different versions of the same package
- Installing or upgrading a package in one project can silently break another
- You have no record of which packages belong to which project

A virtual environment solves this by creating a **project-specific copy** of Python that is completely independent from the global installation.

### The `.venv` Folder

By convention, virtual environments are stored in a folder called `.venv` inside your project directory. The dot prefix (`.`) marks it as a hidden configuration folder—similar to `.git` for version control.

> ⚠️ **Important:** The `.venv` folder should NEVER be committed to version control. It is machine-specific and can be recreated from `requirements.txt` at any time. Your `.gitignore` file should always include `.venv/`.

---

## Hands-On Exercise

### 1. Open PowerShell and Navigate to Your Project

**Steps:**
1. Open Windows PowerShell
2. Navigate to a project folder (or create one):
   ```powershell
   cd ~/Desktop
   mkdir venv-practice
   cd venv-practice
   ```

---

### 2. Create the Virtual Environment

**Justification:** We are creating an isolated workspace for this project. The `python -m venv` command tells Python to use its built-in `venv` module to generate the environment.

**Steps:**
1. Run the following command:
   ```powershell
   python -m venv .venv
   ```
2. Wait a few seconds for the environment to be created
3. Verify the folder was created:
   ```powershell
   ls .venv
   ```
   You should see folders like `Include`, `Lib`, `Scripts`, and a `pyvenv.cfg` file.

---

### 3. Activate the Virtual Environment

**Justification:** Creating the environment does not automatically use it. You must **activate** it to tell PowerShell "use this project's Python, not the global one."

**Steps:**
1. Run the activation command:
   ```powershell
   .venv\Scripts\Activate
   ```
2. You should see `(.venv)` appear at the beginning of your command prompt:
   ```
   (.venv) PS C:\Users\YourName\Desktop\venv-practice>
   ```

> **Note:** You must activate the virtual environment every time you open a new PowerShell window. It does not persist between sessions.

---

### 4. Verify Isolation

**Justification:** We need to prove that the virtual environment is truly isolated from the global Python installation.

**Steps:**
1. Check which Python is being used:
   ```powershell
   python -c "import sys; print(sys.executable)"
   ```
   The output should point to a path inside your `.venv` folder, NOT the global Python path.

2. List installed packages:
   ```powershell
   pip list
   ```
   You should see only `pip` and `setuptools`—a clean slate with no other packages.

---

### 5. Install a Package and Record Dependencies

**Steps:**
1. Install a package (e.g., `requests`):
   ```powershell
   pip install requests
   ```
2. Record all installed packages:
   ```powershell
   pip freeze > requirements.txt
   ```
3. Open `requirements.txt` in Notepad to inspect the contents:
   ```powershell
   notepad requirements.txt
   ```
   You will see an exact list of packages and their version numbers. This is your project's "Bill of Materials."

---

### 6. Deactivate the Virtual Environment

**Steps:**
1. When you are finished working, deactivate the environment:
   ```powershell
   deactivate
   ```
2. The `(.venv)` prefix will disappear from your prompt, confirming you are back in the global Python environment.

---

### 7. Recreate from Requirements (Reproducibility Test)

**Justification:** The ultimate test of a reproducible environment is whether a colleague (or your future self) can recreate it identically from the `requirements.txt` file alone.

**Steps:**
1. Delete the `.venv` folder:
   ```powershell
   Remove-Item -Recurse -Force .venv
   ```
2. Recreate the virtual environment:
   ```powershell
   python -m venv .venv
   .venv\Scripts\Activate
   ```
3. Reinstall all packages from the requirements file:
   ```powershell
   pip install -r requirements.txt
   ```
4. Verify the packages are installed:
   ```powershell
   pip list
   ```

You should see the exact same packages and versions as before. This is the power of reproducibility.

---

## ✅ Verification Checkpoint

| Check | How to Verify | Expected Result |
|-------|--------------|----------------|
| Environment created | `ls .venv` | Folder exists with `Scripts/`, `Lib/`, etc. |
| Activation works | `.venv\Scripts\Activate` | `(.venv)` prefix in prompt |
| Isolation confirmed | `pip list` (fresh env) | Only `pip` and `setuptools` |
| Dependencies recorded | `cat requirements.txt` | Package list with versions |
| Reproducibility | Delete `.venv`, recreate from `requirements.txt` | Identical environment |

---

## Key Takeaways

| Concept | Analogy | Why It Matters |
|---------|---------|---------------|
| Virtual environment | Sterile field | Isolates project dependencies |
| `requirements.txt` | Bill of Materials | Documents every tool and version |
| `.venv` in `.gitignore` | Machine-specific config | Environment is recreated, not shared |
| `pip freeze` | Instrument inventory | Creates the reproducibility record |

You are now ready for **Module: IDE Setup — VS Code**.
