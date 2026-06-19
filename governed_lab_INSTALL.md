# Governed Lab — Installation Guide

## What you need before you start

You need three things installed on your machine before Governed Lab will run.
None of them are included in the zip — they must be installed separately.

---

### 1. Python 3.9 or later

**Check if you already have it:**
```powershell
python --version
```
If you see `Python 3.9.x` or higher, you're good. Skip to step 2.

**If not installed:**
1. Go to https://www.python.org/downloads/
2. Download the latest Python 3.x installer
3. Run the installer
4. **On the first screen, tick "Add Python to PATH"** — this is easy to miss and required
5. Click Install Now

Verify it worked:
```powershell
python --version
pip --version
```
Both should return a version number.

---

### 2. Git

Git is required for the audit commit feature. If you don't have it the
system still runs, but writes won't be committed to version control.

**Check if you already have it:**
```powershell
git --version
```

**If not installed:**
1. Go to https://git-scm.com/download/win
2. Download and run the installer
3. Accept all defaults

After installing, set your identity (required for git commits):
```powershell
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

---

### 3. Ollama (the local AI engine)

Ollama runs the language model locally on your machine.
No internet connection or API key is needed after setup.

**Install:**
1. Go to https://ollama.com
2. Click Download and run the Windows installer
3. Ollama runs as a background service automatically after install

**Pull the model Governed Lab uses:**
```powershell
ollama pull qwen2.5-coder:7b
```
This downloads approximately 4.7 GB. Run it once and it stays on your machine.

**Verify Ollama is running:**
```powershell
ollama list
```
You should see `qwen2.5-coder:7b` in the list.

---

## Installation

### Step 1 — Unzip

Extract `governed_lab.zip` to wherever you keep your projects.
This guide assumes `C:\projects\` but any location works.

After extracting you should have:
```
C:\projects\governed_lab\
    governed_runner.py
    policy_firewall.py
    policy.json
    dashboard.py
    requirements.txt
    workspace\
    sandbox\
    external_projects\
        sample_project\
        todo_cli\
```

---

### Step 2 — Open PowerShell in the project folder

```powershell
cd C:\projects\governed_lab
```

Or right-click the `governed_lab` folder in Explorer and choose
**Open in Terminal**.

---

### Step 3 — Install Python dependencies

```powershell
pip install -r requirements.txt
```

This installs Flask, Requests, and PyYAML. It takes about 30 seconds.
You only need to do this once.

---

### Step 4 — Initialise Git

The governance system commits every approved file change to Git.
Run this once inside the project folder:

```powershell
git init
git add .
git commit -m "Initial governed_lab setup"
```

---

### Step 5 — Verify everything works

```powershell
python governed_runner.py
```

You should see:
```
Governed LLM Sandbox - Automated Coder Ready
Allowed paths: workspace/ external_projects/
```

Type `END` and press Enter to exit.

---

## Running the system

### The code runner (main tool)

```powershell
python governed_runner.py
```

When the prompt appears, type your instruction in this format:

```
FILE: workspace/test.py

CONTENT:
def greet():
    print("Hello world")

REQUEST:
Change the greeting to say Hello Carl.
END
```

The runner calls Ollama, gets a proposed change, runs it through
the governance pipeline, and either writes the file or explains why it was blocked.

---

### The dashboard (audit log viewer)

In a separate PowerShell window:

```powershell
python dashboard.py
```

Open your browser and go to: **http://127.0.0.1:5000**

Login with:
- Username: `admin`
- Password: `changeme`

To set your own password, set environment variables before starting:
```powershell
$env:DASH_USER = "admin"
$env:DASH_PASS = "your-password-here"
python dashboard.py
```

---

## Folder reference

| Folder | Purpose |
|---|---|
| `workspace\` | General scratch area — use for experiments and personal files |
| `sandbox\` | Testing area — for trying things out safely |
| `external_projects\sample_project\` | Example project for tutorials |
| `external_projects\todo_cli\` | Example project for tutorials |
| `logs\` | Audit log (created automatically on first run) |
| `history\` | File snapshots before each change (created automatically) |

The runner can only write to `workspace\` and `external_projects\`.
Any attempt to write outside these folders is blocked by policy.

---

## Troubleshooting

**`python` is not recognised**
→ Python is not on your PATH. Reinstall Python and tick "Add Python to PATH".

**`pip install` fails**
→ Try `python -m pip install -r requirements.txt` instead.

**`ollama` is not recognised**
→ Ollama is not installed or not on PATH. Reinstall from https://ollama.com.

**Runner starts but hangs after END with no output**
→ Ollama is not running. Open a new PowerShell window and run `ollama serve`,
then try again.

**`Cannot connect to Ollama` error**
→ Same as above — run `ollama serve` in a separate window, or restart your machine
(Ollama installs as a background service and should start automatically).

**`qwen2.5-coder:7b` not found error**
→ Run `ollama pull qwen2.5-coder:7b` to download the model.

**Dashboard shows 401**
→ Enter your credentials in the browser login popup.
Default is `admin` / `changeme`.

**Git commit errors on first run**
→ Run `git init` and `git config` steps from Step 4 above.

---

## Quick start checklist

- [ ] Python 3.9+ installed with "Add to PATH" ticked
- [ ] `python --version` returns a version number
- [ ] `pip --version` returns a version number
- [ ] Git installed
- [ ] `git --version` returns a version number
- [ ] Ollama installed from https://ollama.com
- [ ] `ollama pull qwen2.5-coder:7b` completed
- [ ] `ollama list` shows `qwen2.5-coder:7b`
- [ ] Unzipped to your projects folder
- [ ] `pip install -r requirements.txt` completed
- [ ] `git init` and initial commit done
- [ ] `python governed_runner.py` shows the ready prompt
