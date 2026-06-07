# Secrets & Environment Variables

## Overview

In the medical device industry, controlled substances and restricted materials are stored in locked cabinets with access logs. You would never leave a controlled substance on an open countertop.

**Secrets** in software work the same way. A secret is any piece of sensitive information that, if exposed, could grant unauthorized access to systems, data, or services. Common examples include:

- **API keys** — credentials to access external services (e.g., Google Gemini, cloud databases)
- **Passwords** — database or system login credentials
- **Tokens** — temporary access codes for authentication
- **Private keys** — cryptographic credentials for secure communication

> **Why This Matters in Regulated Industries:**
> Leaked secrets can lead to data breaches, unauthorized system access, and regulatory violations. HIPAA, FDA 21 CFR Part 11, and the EU AI Act all require that access credentials are properly managed. In April 2026, the FDA cited a manufacturer for inadequate data access controls in their AI-assisted workflows. Proper secrets management is not optional—it is a compliance requirement.

---

## Concepts

### Environment Variables

An **environment variable** is a value stored outside of your code that your program can read at runtime. Instead of writing:

```python
# ❌ NEVER DO THIS — hardcoded secret
api_key = "sk-abc123xyz789"
```

You store the key in a separate file and read it dynamically:

```python
# ✅ CORRECT — loaded from environment
import os
api_key = os.getenv("GEMINI_API_KEY")
```

This separation ensures that secrets never appear in your source code or Git history.

### The `.env` File

A `.env` file is a simple text file that stores environment variables as key-value pairs:

```
GEMINI_API_KEY=sk-abc123xyz789
DATABASE_URL=postgresql://user:pass@localhost/mydb
DEBUG_MODE=false
```

### The `.env.example` File

A `.env.example` (or `.env.sample`) file is a **template** that shows what variables are needed without revealing actual values:

```
GEMINI_API_KEY=your-api-key-here
DATABASE_URL=your-database-url-here
DEBUG_MODE=false
```

This file IS committed to Git so that team members know which variables they need to configure.

---

## The Golden Rules

| Rule | Why |
|------|-----|
| **Never hardcode secrets** | If it's in code, it's in Git history forever |
| **Never commit `.env`** | Must be in `.gitignore` |
| **Always commit `.env.example`** | Documents required variables without exposing values |
| **Never share secrets via chat/email** | Use a secrets manager or secure transfer |
| **Rotate compromised secrets immediately** | If a key is leaked, generate a new one |

---

## Hands-On Exercise

### 1. Inspect the Workshop's `.env.example`

**Steps:**
1. Open the workshop repository in VS Code (or navigate to it in PowerShell)
2. Open the `.env.example` file:
   ```powershell
   notepad .env.example
   ```
3. Review the variables listed. These are the credentials and settings your project needs, but the actual values are NOT stored in this file.

---

### 2. Create Your Own `.env` File

**Justification:** We will create a local `.env` file with actual (or placeholder) values. This file stays on your machine and is never shared.

**Steps:**
1. Copy the example file:
   ```powershell
   copy .env.example .env
   ```
2. Open `.env` in Notepad and replace placeholder values with your actual credentials (or practice values):
   ```
   GEMINI_API_KEY=your-real-key-here
   ```
3. Save and close

---

### 3. Verify `.env` is Ignored by Git

**Justification:** This is the most critical step. We must prove that Git will NOT track your secrets file.

**Steps:**
1. Check the `.gitignore` file to confirm `.env` is listed:
   ```powershell
   Select-String ".env" .gitignore
   ```
2. Check Git status:
   ```powershell
   git status
   ```
3. The `.env` file should NOT appear in the list of untracked or staged files. If it does, your `.gitignore` is misconfigured.

> ⚠️ **If `.env` appears in `git status`:** Add `.env` to your `.gitignore` file immediately, then run `git status` again to confirm.

---

### 4. Load Secrets in Python Using `python-dotenv`

**Justification:** The `python-dotenv` library automatically reads your `.env` file and makes the values available as environment variables in your Python code.

**Steps:**
1. Activate your virtual environment:
   ```powershell
   .venv\Scripts\Activate
   ```
2. Install the library:
   ```powershell
   pip install python-dotenv
   ```
3. Create a test script called `test_secrets.py`:
   ```python
   import os
   from dotenv import load_dotenv

   # Load variables from .env file
   load_dotenv()

   # Read a variable
   api_key = os.getenv("GEMINI_API_KEY")

   if api_key:
       # Only show the first 4 characters for safety
       masked = api_key[:4] + "..." + api_key[-4:]
       print(f"API key loaded successfully: {masked}")
   else:
       print("WARNING: GEMINI_API_KEY not found in .env file!")
   ```
4. Run the script:
   ```powershell
   python test_secrets.py
   ```

---

### 5. Update Your Requirements

**Steps:**
1. Record the new dependency:
   ```powershell
   pip freeze > requirements.txt
   ```

---

## What Happens If You Accidentally Commit a Secret?

If a secret is accidentally committed to Git:

1. **The secret is compromised** — even if you delete the file, the secret remains in Git history
2. **Immediately rotate the secret** — generate a new API key or password from the service provider
3. **Do NOT just delete the file and commit** — the old value is still in the history
4. **Use `git filter-branch` or BFG Repo Cleaner** to rewrite history (advanced, last resort)

The best protection is prevention: always verify `.gitignore` before your first commit.

---

## ✅ Verification Checkpoint

| Check | How to Verify | Expected Result |
|-------|--------------|----------------|
| `.env.example` exists | `ls .env.example` | File present in project root |
| `.env` created | `ls .env` | File present (with real/practice values) |
| `.env` is git-ignored | `git status` | `.env` does NOT appear |
| `python-dotenv` works | Run `test_secrets.py` | Key loaded and masked output shown |
| No secrets in code | Search `*.py` files for hardcoded keys | No actual API keys found |

---

## Key Takeaways

| Concept | Analogy | Why It Matters |
|---------|---------|---------------|
| `.env` file | Locked cabinet | Stores secrets locally, never shared |
| `.env.example` | Cabinet label | Documents what's needed without exposing contents |
| `.gitignore` | Access restriction list | Prevents secrets from entering the audit trail |
| `python-dotenv` | Key card reader | Loads secrets securely at runtime |
| Secret rotation | Lock change after key loss | Compromised secrets must be replaced immediately |

You are now ready for **Section 2: Command-Line AI & Agentic Workflows**.
