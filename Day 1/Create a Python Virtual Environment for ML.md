# Setting Up a Python Virtual Environment for an ML Project

## Task Overview

The xFusionCorp Industries data science team requires a standardized Python environment for a new Machine Learning project.

### Requirements

The xFusionCorp Industries data science team needs a standardised Python environment for their new ML project. Set up a virtual environment with the required ML libraries on the `controlplane` host.

1. Create a Python virtual environment named `ml-env` under `/root/code/` using `python3 -m venv`.
2. Activate the environment and install the following packages: `numpy`, `pandas`, `scikit-learn`, and `matplotlib`.
3. Generate a `requirements.txt` file using `pip freeze` and save it at `/root/code/requirements.txt`.

---

# Step-by-Step Solution

## Step 1 — Create the Directory and Virtual Environment

Run the following commands:

```bash
# Create the directory if it does not exist
mkdir -p /root/code/

# Move into the directory
cd /root/code/

# Create the virtual environment
python3 -m venv ml-env
```

---

## What Does This Command Mean?

```bash
python3 -m venv ml-env
```

### Explanation

| Part | Meaning |
|---|---|
| `python3` | Uses Python 3 |
| `-m` | Runs a Python module |
| `venv` | Python’s built-in virtual environment module |
| `ml-env` | Name of the virtual environment folder |

### What Happens?

Python creates an isolated environment inside the `ml-env` folder containing:

- A separate Python interpreter
- A dedicated `pip`
- Independent package storage

This prevents conflicts between projects.

---

# Step 2 — Activate the Virtual Environment

Run:

```bash
source ml-env/bin/activate
```

---

## What Happens Here?

The `activate` script modifies your shell environment temporarily.

After activation:

- `python` points to the environment’s Python
- `pip` installs packages only inside `ml-env`

Your terminal prompt changes to:

```bash
(ml-env)
```

This confirms the environment is active.

---

# Step 3 — Install Required ML Libraries

Run:

```bash
pip install numpy pandas scikit-learn matplotlib
```

---

# What Are These Libraries?

## NumPy

Used for:

- Numerical computing
- Arrays and matrices
- Mathematical operations

Example:

```python
import numpy as np
```

---

## Pandas

Used for:

- Data analysis
- Data cleaning
- Working with tables and CSV files

Example:

```python
import pandas as pd
```

---

## Scikit-learn

Used for:

- Machine Learning algorithms
- Model training
- Classification and regression

Example:

```python
from sklearn.linear_model import LinearRegression
```

---

## Matplotlib

Used for:

- Graphs
- Charts
- Data visualization

Example:

```python
import matplotlib.pyplot as plt
```

---

# Step 4 — Generate requirements.txt

Run:

```bash
pip freeze > /root/code/requirements.txt
```

---

# What Does This Command Do?

## `pip freeze`

Lists all installed packages and their exact versions.

Example:

```text
numpy==2.1.0
pandas==2.2.1
```

---

## `>` Redirection Operator

Redirects output into a file.

So this command:

```bash
pip freeze > /root/code/requirements.txt
```

saves the package list into:

```text
/root/code/requirements.txt
```

---

# Verify the File

Run:

```bash
cat /root/code/requirements.txt
```

You should see installed packages and versions.

Example:

```text
matplotlib==3.9.0
numpy==2.1.0
pandas==2.2.2
scikit-learn==1.5.0
```

---

# Deactivate the Environment

When finished, run:

```bash
deactivate
```

This returns you to the system’s default Python environment.

---

# Final Command Summary

```bash
mkdir -p /root/code/

cd /root/code/

python3 -m venv ml-env

source ml-env/bin/activate

pip install numpy pandas scikit-learn matplotlib

pip freeze > /root/code/requirements.txt
```

---

# Key Concepts Learned

| Concept | Description |
|---|---|
| Virtual Environment | Isolated Python workspace |
| `venv` | Python module for creating environments |
| `pip` | Python package manager |
| `pip freeze` | Exports installed packages |
| `requirements.txt` | Stores dependencies and versions |

---

# Why Virtual Environments Are Important

Virtual environments help:

- Avoid package conflicts
- Keep projects isolated
- Improve reproducibility
- Simplify deployment
- Maintain consistent environments across teams

This is considered a best practice in Python, DevOps, Data Science, and MLOps workflows.