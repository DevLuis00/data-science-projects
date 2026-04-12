# Data Science Environment Setup Guide (Cross-Platform)

## Overview

This document provides a standardized procedure to configure a reproducible Data Science and Machine Learning environment using Python 3.11 and `requirements.txt`. It is designed to work across Linux, Windows, and macOS systems.

---

## Requirements

* Python 3.11
* pip (included with Python)
* Terminal (Bash, PowerShell, or CMD)
* Optional: VS Code with Python and Jupyter extensions

---

## Environment Contents

The environment includes:

* numpy
* pandas
* matplotlib
* seaborn
* scikit-learn
* scipy
* jupyter
* notebook
* ipykernel
* ydata-profiling
* openpyxl
* xlrd
* requests

All dependencies are installed via `requirements.txt`.

---

## Linux Setup

### 1. Install Python 3.11

Fedora:

```bash
sudo dnf install python3.11 python3.11-venv python3.11-devel
```

Ubuntu:

```bash
sudo apt update
sudo apt install python3.11 python3.11-venv python3.11-dev
```

### 2. Install Build Tools

Fedora:

```bash
sudo dnf install gcc gcc-c++ make
```

Ubuntu:

```bash
sudo apt install build-essential
```

### 3. Create Virtual Environment

```bash
python3.11 -m venv .venv
```

### 4. Activate Environment

```bash
source .venv/bin/activate
```

### 5. Install Dependencies

```bash
pip install -r requirements.txt
```

### 6. Register Jupyter Kernel

```bash
python -m ipykernel install --user --name entorno_ml
```

---

## Windows Setup

### 1. Install Python 3.11 (via terminal)

Using winget:

```powershell
winget install Python.Python.3.11
```

Verify installation:

```powershell
python --version
```

### 2. Create Virtual Environment

```powershell
python -m venv .venv
```

### 3. Activate Environment

CMD:

```bat
.venv\Scripts\activate
.entorno_ml\Scripts\activate.bat
```

PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

### 4. Install Dependencies

```powershell
pip install -r requirements.txt
```

### 5. Register Jupyter Kernel

```powershell
python -m ipykernel install --user --name entorno_ml
```

---

## macOS Setup

### 1. Install Python 3.11

Using Homebrew:

```bash
brew install python@3.11
```

### 2. Create Virtual Environment

```bash
python3.11 -m venv .venv
```

### 3. Activate Environment

```bash
source .venv/bin/activate
```

### 4. Install Dependencies

```bash
pip install -r requirements.txt
```

### 5. Register Jupyter Kernel

```bash
python -m ipykernel install --user --name entorno_ml
```

---

## Running Jupyter

```bash
jupyter notebook
```

or

```bash
jupyter lab
```

---

## VS Code Integration

1. Open project folder
2. Press `Ctrl + Shift + P`
3. Select `Python: Select Interpreter`
4. Choose `.venv`
5. In notebooks, select kernel `entorno_ml`

---

## Project Structure

```
project/
│
├── data/
├── notebooks/
├── src/
├── requirements.txt
└── .venv/
```

---

## Reproducing the Environment

On any machine:

```bash
python -m venv .venv
```

Activate environment and install dependencies:

```bash
pip install -r requirements.txt
```

---

## Common Issues

### Python not recognized (Windows)

Ensure Python is added to PATH or use:

```powershell
py -3.11 --version
```

---

### Compilation errors (Linux)

Install required tools:

```bash
sudo dnf install gcc python3-devel
```

or

```bash
sudo apt install build-essential
```

---

### Jupyter kernel not visible

```bash
jupyter kernelspec list
```

Reinstall kernel:

```bash
python -m ipykernel install --user --name entorno_ml
```

---

## Best Practices

* Use one virtual environment per project
* Do not share `.venv`
* Always share `requirements.txt`
* Use Python 3.11 for consistency
* Avoid upgrading to incompatible library versions

---

## Conclusion

This setup ensures:

* Reproducibility across systems
* Consistent dependency management
* Compatibility with Data Science and Machine Learning workflows
* Minimal configuration overhead on new machines

---
