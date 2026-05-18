# Enforcing Code Quality with Black and Ruff

Modern Machine Learning projects are rarely built by one person. Usually, multiple:

- Data Scientists
- ML Engineers
- DevOps Engineers
- Backend Developers

work on the same repository.

Without coding standards, the project quickly becomes messy:

- inconsistent formatting
- unused imports
- bad indentation
- different naming styles
- hard-to-read code

To solve this, teams enforce automated code quality checks using tools like:

- Black
- Ruff

These tools are usually integrated into:

- GitHub Actions
- GitLab CI/CD
- Jenkins pipelines
- pre-commit hooks

If the code quality checks fail, the Pull Request cannot be merged.

This task is about configuring and enforcing those standards in the `fraud-detection` project.

---

# Understanding the Tools

---

# 1. Black — The Python Code Formatter

Black is an automatic Python code formatter.

It rewrites Python files into a consistent style.

Instead of warning about formatting issues, it fixes them automatically.

---

## What Black Fixes

Examples:

### Spacing

Before:

```python
x=1+2
```

After:

```python
x = 1 + 2
```

---

### Quotes

Before:

```python
name='dasith'
```

After:

```python
name = "dasith"
```

---

### Long Lines

Black automatically wraps long lines according to the configured line length.

---

# Why Teams Use Black

Without Black:

- every developer formats differently
- Pull Requests become messy
- Git diffs become unreadable

Black enforces one consistent style for the entire team.

---

# 2. Ruff — The Fast Python Linter

Ruff is a modern Python linter written in Rust.

It is extremely fast.

While Black focuses on formatting, Ruff focuses on:

- bugs
- warnings
- bad practices
- import cleanup
- unused variables
- unused imports

---

# Example Ruff Problems

---

## Unused Import

```python
import pandas
```

but never using it.

Ruff detects this automatically.

---

## Unused Variable

```python
x = 100
```

but never using `x`.

Ruff flags it.

---

## Import Sorting

Ruff can also automatically organize imports.

Before:

```python
import pandas
import os
import numpy
```

After:

```python
import os

import numpy
import pandas
```

---

# What Does "Exit Status 0" Mean?

In Linux and CI/CD systems:

```text
Exit Code 0 = Success
```

Any non-zero exit code means failure.

Examples:

| Exit Code | Meaning |
|---|---|
| 0 | Success |
| 1 | Error |
| 2 | Misuse or syntax issue |

CI/CD pipelines use these exit codes to decide:

- pass
- fail
- block deployment

---

# The Goal of This Task

Ensure these commands pass successfully:

```bash
black --check src/

ruff check src/
```

Both commands must exit successfully with status `0`.

---

# Step 1 — Open the Project

Move into the project:

```bash
cd /root/code/fraud-detection/
```

---

# Step 2 — Open pyproject.toml

Open:

```bash
pyproject.toml
```

using VS Code or nano.

Example with nano:

```bash
nano pyproject.toml
```

---

# What Is pyproject.toml?

This is the central configuration file for modern Python tooling.

Many tools read configuration from it:

- Black
- Ruff
- Poetry
- Pytest
- Build systems

It acts like a global project settings file.

---

# Step 3 — Configure Black and Ruff

Add or update the following configuration:

```toml
[tool.black]
line-length = 120

[tool.ruff]
line-length = 120

[tool.ruff.lint]
select = ["E", "F", "W", "I"]
```

Save the file.

---

# Understanding the Configuration

---

# Black Configuration

```toml
[tool.black]
line-length = 120
```

This tells Black:

> Maximum allowed line length is 120 characters.

---

# Ruff Configuration

```toml
[tool.ruff]
line-length = 120
```

Ensures Ruff follows the same line-length standard.

Consistency matters.

---

# Ruff Lint Rules

```toml
select = ["E", "F", "W", "I"]
```

These enable specific rule categories.

---

## E — Errors

Detects syntax and formatting errors.

---

## F — Flake/Bug Rules

Detects:

- unused variables
- undefined names
- logical issues

---

## W — Warnings

Detects risky or suspicious code patterns.

---

## I — Import Sorting

Automatically organizes imports.

Similar to `isort`.

---

# Step 4 — Automatically Fix the Code

Instead of manually editing every file, let the tools fix the code automatically.

---

# Run Black

```bash
black src/
```

This reformats every Python file inside `src/`.

---

# What Happens?

Black automatically:

- fixes spacing
- wraps long lines
- standardizes quotes
- reformats code

---

# Run Ruff with Auto-Fix

```bash
ruff check src/ --fix
```

---

# What Happens?

Ruff automatically fixes:

- unused imports
- import ordering
- simple linting problems

---

# Why This Is Powerful

In large projects with hundreds of files:

manual cleanup is impossible.

Automation saves enormous amounts of engineering time.

---

# Step 5 — Verify the Fixes

Now run the same commands the CI/CD pipeline will execute.

---

# Check Black

```bash
black --check src/
```

---

# Check Ruff

```bash
ruff check src/
```

---

# Expected Result

You should see either:

```text
All done!
```

or minimal success output.

No errors should appear.

This means both tools exited successfully with:

```text
Exit Status 0
```

---

# Why CI/CD Pipelines Use These Checks

In real production systems:

every Pull Request automatically runs:

- tests
- formatting checks
- linting checks
- security scans

If any check fails:

- merge is blocked
- deployment stops

This prevents bad code from entering production.

---

# Real-World Workflow

A developer writes code.

Before merging:

```bash
black src/
ruff check src/ --fix
pytest
```

Then CI/CD validates:

```bash
black --check src/
ruff check src/
```

If everything passes:

- Pull Request is approved
- deployment continues

---

# Why Standardization Matters

Without standards:

- debugging becomes difficult
- code reviews become painful
- onboarding new developers becomes slower
- merge conflicts increase

With automated tooling:

- code becomes predictable
- teams collaborate efficiently
- repositories stay clean

---

# Final Commands Summary

---

## Configure pyproject.toml

```toml
[tool.black]
line-length = 120

[tool.ruff]
line-length = 120

[tool.ruff.lint]
select = ["E", "F", "W", "I"]
```

---

## Automatically Fix Formatting

```bash
black src/
```

---

## Automatically Fix Linting Issues

```bash
ruff check src/ --fix
```

---

## Verify CI/CD Checks

```bash
black --check src/

ruff check src/
```

---

# Key Concepts Learned

| Tool | Purpose |
|---|---|
| Black | Automatic code formatter |
| Ruff | Fast Python linter |
| pyproject.toml | Python tool configuration |
| Linting | Static code analysis |
| CI/CD | Automated quality enforcement |
| Exit Status 0 | Successful execution |

This workflow is one of the most important foundations of professional Python, DevOps, and MLOps engineering.