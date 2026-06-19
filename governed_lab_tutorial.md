# Governed Lab — Hands-On Tutorial

This tutorial walks through every major operation the system supports,
using your four real project areas:

- `workspace/`
- `sandbox/`
- `external_projects/sample_project/`
- `external_projects/todo_cli/`

Each step tells you exactly what to type and what to expect back.

---

## Before you start

Open **two PowerShell windows**, both in `C:\projects\governed_lab`.

- **Window 1** — you run `governed_runner.py` here
- **Window 2** — you check files here with `cat` and `ls`

---

## PART 1 — Create folders and files

### 1A — Create a file in workspace

**Window 2 — create the folder structure first:**
```powershell
mkdir workspace\utils
```

**Window 1 — start the runner:**
```powershell
python governed_runner.py
```

**Type this input, then press Enter after END:**
```
FILE: workspace/utils/math_tools.py

CONTENT:
def add(a, b):
    return a + b

REQUEST:
This is a new file. Keep the add function exactly as shown.
END
```

**Expected output:**
```
[GIT] Committed: workspace/utils/math_tools.py
REASON: ...
[OK] Updated: C:\projects\governed_lab\workspace\utils\math_tools.py
```

**Window 2 — verify:**
```powershell
cat workspace\utils\math_tools.py
```

---

### 1B — Create a file in sandbox

**Window 2:**
```powershell
mkdir sandbox
```

**Window 1 — start the runner:**
```powershell
python governed_runner.py
```

**Input:**
```
FILE: sandbox/greeter.py

CONTENT:
def greet(name):
    return f"Hello, {name}!"

REQUEST:
This is a new file. Keep the greet function exactly as shown.
END
```

**Window 2 — verify:**
```powershell
cat sandbox\greeter.py
```

---

### 1C — Create a file in external_projects/sample_project

**Window 2:**
```powershell
mkdir external_projects\sample_project
```

**Window 1:**
```powershell
python governed_runner.py
```

**Input:**
```
FILE: external_projects/sample_project/calculator.py

CONTENT:
def multiply(a, b):
    return a * b

REQUEST:
This is a new file. Keep the multiply function exactly as shown.
END
```

**Window 2 — verify:**
```powershell
cat external_projects\sample_project\calculator.py
```

---

### 1D — Create a file in external_projects/todo_cli

**Window 2:**
```powershell
mkdir external_projects\todo_cli
```

**Window 1:**
```powershell
python governed_runner.py
```

**Input:**
```
FILE: external_projects/todo_cli/tasks.py

CONTENT:
tasks = []

def add_task(name):
    tasks.append(name)

def list_tasks():
    return tasks

REQUEST:
This is a new file. Keep both functions exactly as shown.
END
```

**Window 2 — verify:**
```powershell
cat external_projects\todo_cli\tasks.py
```

---

## PART 2 — Change file content

### 2A — Modify a function in workspace

**Window 1:**
```powershell
python governed_runner.py
```

**Input:**
```
FILE: workspace/utils/math_tools.py

CONTENT:
def add(a, b):
    return a + b

REQUEST:
Add a subtract(a, b) function that returns a minus b.
END
```

**Window 2 — verify both functions are present:**
```powershell
cat workspace\utils\math_tools.py
```

**Expected file content:**
```python
def add(a, b):
    return a + b

def subtract(a, b):
    return a - b
```

---

### 2B — Modify a function in sandbox

**Window 1:**
```powershell
python governed_runner.py
```

**Input:**
```
FILE: sandbox/greeter.py

CONTENT:
def greet(name):
    return f"Hello, {name}!"

REQUEST:
Change the greeting to say Good morning instead of Hello.
END
```

**Window 2 — verify:**
```powershell
cat sandbox\greeter.py
```

---

### 2C — Modify a function in external_projects/sample_project

**Window 1:**
```powershell
python governed_runner.py
```

**Input:**
```
FILE: external_projects/sample_project/calculator.py

CONTENT:
def multiply(a, b):
    return a * b

REQUEST:
Add a divide(a, b) function that returns a divided by b,
and raises ValueError if b is zero.
END
```

**Window 2 — verify:**
```powershell
cat external_projects\sample_project\calculator.py
```

---

### 2D — Modify a function in external_projects/todo_cli

**Window 1:**
```powershell
python governed_runner.py
```

**Input:**
```
FILE: external_projects/todo_cli/tasks.py

CONTENT:
tasks = []

def add_task(name):
    tasks.append(name)

def list_tasks():
    return tasks

REQUEST:
Add a remove_task(name) function that removes the first matching
task from the list, or raises ValueError if the task is not found.
END
```

**Window 2 — verify:**
```powershell
cat external_projects\todo_cli\tasks.py
```

---

## PART 3 — Rename files and folders

The governed runner writes file content — it does not move or rename files
directly. Renaming is a two-step process: create the new file via the runner,
then delete the old file manually in PowerShell.

### 3A — Rename a file (greeter.py → welcome.py)

**Step 1 — read the current content:**
```powershell
cat sandbox\greeter.py
```
Copy the content shown.

**Step 2 — Window 1, create the new filename:**
```powershell
python governed_runner.py
```

**Input (paste the content you copied into CONTENT:):**
```
FILE: sandbox/welcome.py

CONTENT:
def greet(name):
    return f"Good morning, {name}!"

REQUEST:
This is a renamed copy of greeter.py. Keep all functions exactly as shown.
END
```

**Step 3 — Window 2, delete the old file:**
```powershell
Remove-Item sandbox\greeter.py
ls sandbox\
```

You should now see `welcome.py` and no `greeter.py`.

---

### 3B — Rename a folder (utils → helpers)

**Step 1 — Window 2, copy the folder contents:**
```powershell
mkdir workspace\helpers
copy workspace\utils\math_tools.py workspace\helpers\math_tools.py
```

**Step 2 — verify the copy:**
```powershell
cat workspace\helpers\math_tools.py
```

**Step 3 — delete the old folder:**
```powershell
Remove-Item -Recurse workspace\utils
ls workspace\
```

You should now see `helpers\` and no `utils\`.

> Note: you can also do the copy step via the runner if you want the
> operation logged in the audit trail — create a new FILE: block at the
> new path with the same content.

---

## PART 4 — Delete files and folders

The governed runner does not delete files — deletion is intentionally
outside its write contract (a safety design decision: the LLM cannot
remove code it cannot see). Deletion is done directly in PowerShell.

### 4A — Delete a single file

```powershell
Remove-Item sandbox\welcome.py
ls sandbox\
```

---

### 4B — Delete a folder and all its contents

```powershell
Remove-Item -Recurse external_projects\sample_project
ls external_projects\
```

---

### 4C — Delete and recreate (clean slate)

This is the recommended pattern when you want to start a project area fresh:

```powershell
# Delete
Remove-Item -Recurse external_projects\todo_cli

# Recreate
mkdir external_projects\todo_cli
```

Then use the runner to populate it again as in Part 1.

---

## PART 5 — Security tests (what the system blocks)

These inputs should all be rejected. Run each one and confirm you see
a `[SECURITY]` or `[POLICY]` message and no `[OK]`.

### 5A — Path outside allowed roots

**Window 1:**
```powershell
python governed_runner.py
```

**Input:**
```
FILE: C:/Windows/system32/evil.py

CONTENT:
print("hello")

REQUEST:
Change the print message.
END
```

**Expected:** `[SECURITY] Path outside allowed roots`

---

### 5B — Dangerous function (eval)

**Window 1:**
```powershell
python governed_runner.py
```

**Input:**
```
FILE: workspace/test_evil.py

CONTENT:
def safe():
    return 1

REQUEST:
Add a function run_code(user_input) that calls eval(user_input) and returns the result.
END
```

**Expected:** `[POLICY] Violations detected` — `Dangerous call blocked (AST): eval()`

---

### 5C — Full file rewrite attack

**Window 1:**
```powershell
python governed_runner.py
```

**Input:**
```
FILE: workspace/utils/math_tools.py

CONTENT:
# This replaces everything
pass
pass
pass
pass
pass
pass
pass
pass
pass
pass
pass
pass
pass
pass
pass
pass
pass
pass
pass
pass
pass

REQUEST:
Replace the entire file with this content.
END
```

**Expected:** `[SECURITY] Patch removes N existing lines — exceeds hard limit`

---

## Audit log — check everything that happened

After completing the tutorial, check the full audit trail:

```powershell
cat logs\audit_log.jsonl
```

Every approved write and every rejection is recorded here with a timestamp,
file path, and hash before/after.

Or open the dashboard to see it formatted:

```powershell
python dashboard.py
```

Then open `http://127.0.0.1:5000` in your browser (login: admin / changeme).

---

## Quick reference

| What you want to do | How |
|---|---|
| Create a new file | Runner — FILE: at the new path, CONTENT: with initial code |
| Edit file content | Runner — FILE: + CONTENT: (current) + REQUEST: (change) |
| Rename a file | Runner creates new path → PowerShell deletes old path |
| Rename a folder | PowerShell copies contents → PowerShell deletes old folder |
| Delete a file | PowerShell: `Remove-Item path\file.py` |
| Delete a folder | PowerShell: `Remove-Item -Recurse path\folder` |
| Check audit log | `cat logs\audit_log.jsonl` or the dashboard |
| See what's in a file | `cat path\file.py` |
| List folder contents | `ls path\` |

