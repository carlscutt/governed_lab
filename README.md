Governator

A Secure LLM Execution & Governance Platform

Overview

Governator is a defensive execution engine designed to protect enterprise environments from unsafe or unverified LLM-generated code changes.

Rather than trusting model output directly, Governator enforces external governance controls including structural validation, runtime validation, integrity protection, and Git isolation.

Core Principle:
Governance must exist outside the model to prevent mission drift, prompt dilution, and unsafe execution.

Why Governator Exists

Large language models can:

Drift from original intent

Produce syntactically invalid code

Introduce runtime failures

Modify unintended sections of a file

Be vulnerable to prompt injection

Be exploited via time-of-check/time-of-use (TOCTOU) attacks

Governator mitigates these risks through deterministic enforcement layers.

v1.0 Capabilities
1. Injection Scanning

Pre-validates input prompts before model execution.

2. Controlled Patch Extraction

Only applies explicitly defined REPLACE blocks.

3. AST Structural Validation

Ensures syntactic correctness before committing changes.

4. Runtime Validation

Executes updated file in sandbox to detect runtime errors.

5. SHA256 File Hashing

Prevents integrity tampering during execution.

6. TOCTOU Protection

Aborts execution if file changes mid-process.

7. Git Isolation

Creates a new branch per approved change.

8. Audit Logging

Records:

Timestamp

File path

Approval status

Hash before

Hash after

9. CLI Interface

Command-line execution with approval gating.

Architecture Philosophy

Governator enforces a strict separation:

LLM Responsibility	Governator Responsibility
Suggest change	Validate & control execution
Generate diff	Enforce structure
Propose update	Verify safety
Provide patch	Commit with isolation

The model suggests.
Governator decides.

Installation
Clone Repository
git clone <repo-url>
cd governed_lab
Install Dependencies
pip install -r requirements.txt

Dependencies include:

pyyaml

Usage

Basic command:

python governed_runner.py \
  --file sandbox/test.py \
  --request "Modify greet() to print 'Hello Carl'."

Optional:

--auto-approve
Validation Layers

Execution pipeline:

Injection scan

Model request

Output validation

Patch extraction

AST validation

Runtime validation

Integrity verification

Git branch commit

Audit log entry

Failure at any stage results in rollback.

Example Failure Protection
Syntax Failure

Change rejected by AST validator.

Runtime Failure

Change reverted automatically.

Integrity Tampering

Execution aborted.

Dirty Git Working Tree

Commit blocked.

Current Scope (v1.0)

Single-file governance

Replace-based patch control

Deterministic validation flow

Future versions may include:

Multi-file diff governance

Risk scoring

Policy severity levels

Enterprise dashboard integration

Intended Use

Governator is built as:

A portfolio differentiator

A governance demonstration tool

A secure LLM execution sandbox

A foundation for future SaaS development

License

Private project. Enterprise governance research and portfolio use.

#
Logging into the dashboard.

Run the governed_lab_dashboard.exe

or

run Python dashboard.py 

user:admin
pass: changeme

.
