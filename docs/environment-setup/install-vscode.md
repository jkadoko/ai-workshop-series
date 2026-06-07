# IDE Setup — Visual Studio Code

## Overview

So far, we have been writing Python scripts in Notepad and executing them by dragging files into PowerShell. This works for simple exercises, but it is like performing surgery with a flashlight and a pocket knife—technically possible, but far from ideal.

**Visual Studio Code (VS Code)** is a free, professional-grade code editor developed by Microsoft. It transforms your laptop into a proper development workstation by providing:

- **Syntax highlighting** — Python code is color-coded so you can spot errors instantly
- **Integrated terminal** — Run scripts without leaving the editor
- **IntelliSense** — Auto-complete suggestions as you type, like a smart spell-checker for code
- **Version control** — Built-in Git integration for tracking changes
- **Extensions** — Add-on tools tailored to your workflow (Python, AI assistants, etc.)

> **Why This Matters in Regulated Industries:**
> Professional development environments provide consistency, traceability, and error prevention. Just as a surgeon uses a proper operating theater rather than a kitchen table, engineers in regulated industries should use tools designed for professional software development. VS Code's integrated Git support and extension ecosystem make it the standard for modern engineering teams.

---

## Installation Steps

### 1. Install VS Code Using Winget

**Justification:** We use `winget` (Windows Package Manager) because it provides a verified, standardized installation—similar to how we used the Microsoft Store for Python. This avoids downloading from unknown websites.

**Steps:**
1. Open Windows PowerShell
2. Type the following command and press **Enter**:
   ```powershell
   winget install Microsoft.VisualStudioCode
   ```
3. Follow any on-screen prompts to complete the installation
4. **Important:** Close and reopen PowerShell after installation so the system recognizes the new software

---

### 2. Open VS Code

**Steps:**
1. Click the Windows **Start menu**
2. Type "Visual Studio Code"
3. Press **Enter**
4. VS Code will open with a Welcome tab

> **Note:** You can also launch VS Code from PowerShell by typing `code` and pressing Enter.

---

### 3. Install Essential Extensions

**Justification:** Extensions are like specialized instruments—each one adds a specific capability. We will install the minimum set needed for Python development and version control.

**Steps:**
1. In VS Code, click the **Extensions** icon in the left sidebar (it looks like four squares)
2. Search for and install each of the following:

| Extension | Publisher | Purpose |
|-----------|-----------|---------|
| **Python** | Microsoft | Python language support, debugging, IntelliSense |
| **Pylance** | Microsoft | Advanced type checking and code analysis |
| **GitLens** | GitKraken | Enhanced Git history and change tracking |

3. After installing each extension, you may see a "Reload Required" button—click it to activate the extension

---

### 4. Open a Project Folder (Not Individual Files)

**Justification:** Professional development works with **project folders**, not individual files. Opening a folder tells VS Code to treat everything inside as a single project, enabling features like virtual environment detection, Git integration, and relative file references.

**Steps:**
1. In VS Code, click **File** > **Open Folder...**
2. Navigate to a project folder (e.g., the `venv-practice` folder you created in the previous module, or this workshop's repository folder)
3. Click **Select Folder**
4. The left sidebar will now show all files and folders in your project

> ⚠️ **Common Mistake:** Opening individual `.py` files instead of the project folder. This disables most of VS Code's professional features. Always open the **folder**.

---

### 5. Select Your Python Interpreter

**Justification:** If you have multiple versions of Python or multiple virtual environments, VS Code needs to know which one to use. Selecting the correct interpreter ensures your code runs in the right environment.

**Steps:**
1. Press `Ctrl+Shift+P` to open the **Command Palette**
2. Type "Python: Select Interpreter"
3. Select the interpreter that points to your virtual environment (e.g., `.venv\Scripts\python.exe`)
4. You should see the selected interpreter displayed in the bottom status bar

---

### 6. Use the Integrated Terminal

**Justification:** The integrated terminal runs directly inside VS Code, eliminating the need to switch between windows. It automatically activates your virtual environment when configured correctly.

**Steps:**
1. Press `` Ctrl+` `` (backtick key, usually above Tab) to open the terminal
2. The terminal panel appears at the bottom of VS Code
3. If your project has a `.venv`, VS Code may automatically activate it (you will see `(.venv)` in the prompt)
4. Try running a Python script directly:
   ```powershell
   python test_script.py
   ```

---

### 7. Create and Edit a Python File

**Steps:**
1. In the left sidebar, right-click on your project folder and select **New File**
2. Name it `hello_vscode.py`
3. Type the following code:
   ```python
   def greet(name: str) -> str:
       """Return a personalized greeting."""
       return f"Hello, {name}! VS Code is configured correctly."

   if __name__ == "__main__":
       print(greet("Engineer"))
   ```
4. Notice the syntax highlighting, auto-indentation, and type hints
5. Click the **▶ Run** button in the top-right corner (or press `F5` to debug)
6. The output should appear in the integrated terminal

---

## VS Code vs. Notepad: Why It Matters

| Feature | Notepad | VS Code |
|---------|---------|---------|
| Syntax highlighting | ❌ | ✅ |
| Error detection | ❌ | ✅ (real-time) |
| Auto-complete | ❌ | ✅ (IntelliSense) |
| Integrated terminal | ❌ | ✅ |
| Git version control | ❌ | ✅ (built-in) |
| Virtual env support | ❌ | ✅ (auto-detects) |
| Debugging tools | ❌ | ✅ (breakpoints, variables) |
| Extensions/plugins | ❌ | ✅ (thousands available) |

---

## ✅ Verification Checkpoint

| Check | How to Verify | Expected Result |
|-------|--------------|----------------|
| VS Code installed | Type `code` in PowerShell | VS Code launches |
| Python extension | Extensions sidebar | "Python" by Microsoft is installed |
| Project folder open | Left sidebar | Shows project files and folders |
| Interpreter selected | Bottom status bar | Shows `.venv` Python path |
| Terminal works | `` Ctrl+` `` | Terminal opens inside VS Code |
| Script runs | Click ▶ or `F5` | Output in terminal panel |

You are now ready for **Module: Version Control with Git**.
