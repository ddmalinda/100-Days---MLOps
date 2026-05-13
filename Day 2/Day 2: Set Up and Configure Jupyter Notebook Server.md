# JupyterLab Configuration Troubleshooting Guide

## Objective

A JupyterLab server was already configured for the xFusionCorp Industries data science team, but it is not working correctly.

Your task is to:

A teammate has configured a JupyterLab server for the xFusionCorp Industries data science team, but the server does not behave correctly. Inspect the configuration, diagnose the issues, and start the server.


JupyterLab is already installed in the virtual environment at `/root/code/ml-env/`. The team's configuration file is at `/root/code/jupyter_lab_config.py` and is visible in the file explorer.

When JupyterLab is started, the Jupyter UI button at the top of the lab must open the notebook interface.

For this to work, the running server must meet the following requirements:

it listens on port `8888`;
it binds on `0.0.0.0` (the lab proxy cannot reach a server that is only bound on `127.0.0.1)`;
the notebook root directory is /root/notebooks/, and that directory exists on disk.
Open the configuration file, identify every setting that prevents the requirements above from being met, and correct it. Create any missing directories.

Start JupyterLab from the virtual environment using the corrected configuration:

`source /root/code/ml-env/bin/activate`

`jupyter lab --config=/root/code/jupyter_lab_config.py --allow-root --no-browser &`

Make sure JupyterLab is running before using the button at the top of the lab.

---

# Problem Requirements

The running JupyterLab server **must** satisfy all of these conditions:

| Requirement | Value |
|---|---|
| Port | `8888` |
| Bind Address | `0.0.0.0` |
| Notebook Root Directory | `/root/notebooks/` |

Additionally:

- `/root/notebooks/` must exist
- JupyterLab must start using the provided configuration file
- The Jupyter UI button should successfully open the notebook interface

---

# Why the Server Fails

This is a common DevOps and infrastructure troubleshooting issue.

Typical causes include:

- Wrong IP binding (`127.0.0.1`)
- Incorrect port
- Missing notebook directory
- Wrong root directory path

By default, Jupyter binds to:

```python
127.0.0.1
```

This only allows local machine access.

However, lab environments and reverse proxies require:

```python
0.0.0.0
```

which means:

> "Accept connections from all network interfaces."

---

# Step 1 — Create the Notebook Directory

The notebook root directory must exist before Jupyter starts.

Run:

```bash
mkdir -p /root/notebooks/
```

---

## Explanation

### `mkdir`
Creates directories.

### `-p`
Creates parent directories if missing and avoids errors if the directory already exists.

After running this command, the required notebook directory becomes available.

---

# Step 2 — Open the Configuration File

The configuration file is located at:

```bash
/root/code/jupyter_lab_config.py
```

Open it using:

```bash
nano /root/code/jupyter_lab_config.py
```

You can also use:

```bash
vim /root/code/jupyter_lab_config.py
```

or edit it through the file explorer if available.

---

# Step 3 — Fix the Incorrect Settings

Search for the following settings inside the file.

They may:

- already exist with wrong values
- be commented out using `#`

Update them exactly as follows:

```python
# Bind JupyterLab to all interfaces
c.ServerApp.ip = '0.0.0.0'

# Run on port 8888
c.ServerApp.port = 8888

# Set notebook root directory
c.ServerApp.root_dir = '/root/notebooks/'
```

---

# Explanation of Each Setting

---

## 1. `c.ServerApp.ip`

```python
c.ServerApp.ip = '0.0.0.0'
```

### Purpose

Defines which network interfaces Jupyter listens on.

### Why `127.0.0.1` is Wrong

```python
127.0.0.1
```

means:

> "Only allow connections from this machine."

The lab proxy cannot access it.

### Why `0.0.0.0` is Correct

```python
0.0.0.0
```

means:

> "Accept connections from anywhere."

This allows the lab environment proxy to connect properly.

---

## 2. `c.ServerApp.port`

```python
c.ServerApp.port = 8888
```

### Purpose

Defines the port JupyterLab listens on.

The environment expects JupyterLab on:

```text
8888
```

If another port is used, the UI button will fail.

---

## 3. `c.ServerApp.root_dir`

```python
c.ServerApp.root_dir = '/root/notebooks/'
```

### Purpose

Sets the default notebook workspace directory.

When users open JupyterLab, this becomes the main working directory.

---

# Step 4 — Save the File

If using nano:

Press:

```text
CTRL + O
```

then:

```text
Enter
```

to save.

Exit using:

```text
CTRL + X
```

---

# Step 5 — Activate the Python Virtual Environment

JupyterLab is already installed inside the virtual environment:

```text
/root/code/ml-env/
```

Activate it using:

```bash
source /root/code/ml-env/bin/activate
```

---

# Explanation

A Python virtual environment isolates packages and dependencies.

Without activation:

- JupyterLab may not be found
- Wrong Python packages may load

After activation, your shell should show:

```bash
(ml-env)
```

---

# Step 6 — Start JupyterLab

Run:

```bash
jupyter lab --config=/root/code/jupyter_lab_config.py --allow-root --no-browser &
```

---

# Command Breakdown

---

## `jupyter lab`

Starts the JupyterLab server.

---

## `--config`

```bash
--config=/root/code/jupyter_lab_config.py
```

Tells Jupyter to use the corrected configuration file.

---

## `--allow-root`

By default, Jupyter blocks running as root for security reasons.

Since this environment uses the root user, this flag is required.

---

## `--no-browser`

Prevents Jupyter from trying to launch a browser inside the terminal environment.

Useful for remote servers and containers.

---

## `&`

Runs the process in the background.

This keeps the terminal usable while Jupyter continues running.

---

# Step 7 — Verify the Server

Wait a few seconds.

You should see logs similar to:

```text
http://0.0.0.0:8888/lab
```

or:

```text
Jupyter Server is running
```

---

# Optional Verification Commands

## Check Running Process

```bash
ps -ef | grep jupyter
```

---

## Check Listening Port

```bash
ss -tulnp | grep 8888
```

Expected output should show:

```text
0.0.0.0:8888
```

---

# Final Result

After completing all steps:

- JupyterLab runs successfully
- The proxy can access the server
- The notebook UI opens correctly
- The notebook root directory is properly configured

---

# Complete Command Summary

## Create Directory

```bash
mkdir -p /root/notebooks/
```

---

## Edit Config

```bash
nano /root/code/jupyter_lab_config.py
```

Add:

```python
c.ServerApp.ip = '0.0.0.0'
c.ServerApp.port = 8888
c.ServerApp.root_dir = '/root/notebooks/'
```

---

## Activate Virtual Environment

```bash
source /root/code/ml-env/bin/activate
```

---

## Start JupyterLab

```bash
jupyter lab --config=/root/code/jupyter_lab_config.py --allow-root --no-browser &
```

---

# DevOps Concepts Learned

This task teaches important DevOps troubleshooting concepts:

- Service configuration management
- Network binding
- Port configuration
- Reverse proxy compatibility
- Python virtual environments
- Background processes
- Infrastructure debugging
- Server accessibility

These are very common real-world DevOps and MLOps skills.
