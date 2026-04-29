# Claude Code Prompt Patterns

This document defines the structured prompt patterns used in this repository
when working with AI coding assistants such as Claude Code.

The goal is to ensure:

- Safe AI-assisted development
- Evidence-based debugging
- Controlled code generation
- Incremental engineering workflow
- Minimal unintended changes in the repository

These patterns are used during development, testing, and debugging phases.

---

# Development Workflow Loop

The development process follows this cycle:

Health Check → Step Prompt → Checkpoint Review → Commit → Repeat

Additional branches in the workflow:

Audit Mode → used when resuming work  
Diagnostic Mode → used when investigating failures  
Fix Mode → used for multi-file fixes  
Show Me The Evidence → used when validating AI claims  

---

# 1. Health Check

### Purpose
Verify that the environment and repository are in a stable state before making changes.

### When to Use

- Start of every development session
- After pulling new code
- After dependency updates
- Before running new tests

### Example Prompt


Health check and readiness report. Do not modify anything.

Run these commands in order and report raw output:

<environment status command>
<run test suite>
<run smoke test>
git status

### Expected Output

- Environment status
- Test results
- Repository cleanliness
- Readiness for next step

---

# 2. Step Prompt (Primary Development Pattern)

### Purpose
Execute a single, controlled development step.

### When to Use

- Creating a new test file
- Implementing one feature
- Updating one module
- Performing a small code change

### Example Prompt


Step N: <one-sentence deliverable>

Files to create or modify:

<file path>

Reference:

CLAUDE.md
TODO.md
Related module

Requirements:

deterministic tests
no external dependencies
minimal file changes

Checkpoint before exit test:

Show summary of changes
Show git diff
Show git status

### Safety Rules

- Only modify specified files
- No external libraries unless approved
- Stop after checkpoint

---

# 3. Audit Mode

### Purpose
Verify the repository state before continuing development.

### When to Use

- Starting a new session
- Resuming after a break
- Reviewing previous work
- Confirming step completion

### Example Prompt


Audit mode — do not generate code.

Read CLAUDE.md and TODO.md.

Check:

Step completion
Environment persistence
Documentation state
Repository structure

### Output

- Environment health
- Step completion report
- Documentation alignment
- Readiness for next step

---

# 4. Diagnostic Mode

### Purpose
Investigate errors before attempting fixes.

### When to Use

- Build failures
- Test failures
- Unexpected runtime errors
- Conflicting outputs

### Example Prompt


Diagnostic mode — do not fix anything yet.

Error observed:
<command>
<error output>

Run the following commands and return raw output:

<command>
<command>
<command>

### Output

- Raw command outputs
- Root cause diagnosis
- Fix options
- Recommended approach

---

# 5. Fix Mode with Phases

### Purpose
Safely apply fixes across multiple files.

### When to Use

- Configuration changes
- Dependency updates
- Multi-module fixes
- Environment recovery

### Example Prompt


Fix mode — goal: <short description>

Step A:
Run <command>

Step B:
Run <command>

Step C:
Run <verification command>


### Rules

- Execute steps sequentially
- Stop on errors
- Await confirmation before next step

---

# 6. Show Me The Evidence

### Purpose
Verify AI-generated claims using raw command output.

### When to Use

- AI reports success without evidence
- Debugging ambiguous results
- Verifying test execution
- Confirming environment state

### Example Prompt


Before proceeding, show raw output.

Run:

<command>
<command>
<command>

### Rules

- No summarization
- No interpretation
- Raw output only

---

# Development Principles

### Evidence First

Decisions must be based on:

- command output
- repository state
- documentation

AI summaries should not be trusted without verification.

---

### Small Changes

Each development cycle should produce:

- one logical change
- one commit
- one verification step

---

### TODO Tracking

Issues discovered during development must be recorded in:


TODO.md


Use:


[ ] pending
[x] completed


---

# Repository References

- `CLAUDE.md` – AI interaction rules
- `TODO.md` – project memory and open issues
- `README.md` – project overview

---

# Author

Vamsi Kolla  
QA Automation Engineer | SDET  
Focus: Test Automation, API Testing, and AI-assisted development
