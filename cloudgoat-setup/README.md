

# CloudGoat Setup & Debugging (Python 3.11 + Poetry)

CloudGoat is a vulnerable-by-design AWS environment by Rhino Security Labs, used for cloud security training and attack simulations.

This documentation captures:
- The setup process on Ubuntu VM
- Common errors faced (with screenshots)
- How they were resolved
- Final working CLI output

---

## 🖥️ Environment

- OS: Ubuntu 22.04 (VM)
- Python: 3.11
- Poetry: Installed in a virtual environment
- Repo: [CloudGoat](https://github.com/RhinoSecurityLabs/cloudgoat)

---

## 🔧 Setup Steps

1. **Clone the repo**
   ```bash
   git clone https://github.com/RhinoSecurityLabs/cloudgoat.git
   cd cloudgoat
   ```

2. **Upgrade Python using deadsnakes PPA**
   ```bash
   sudo add-apt-repository ppa:deadsnakes/ppa
   sudo apt update
   sudo apt install python3.11 python3.11-venv python3.11-dev
   ```

3. **Install Poetry**
   ```bash
   curl -sSL https://install.python-poetry.org | python3.11 -
   export PATH="$HOME/.local/bin:$PATH"
   ```

4. **Configure Poetry to use Python 3.11**
   ```bash
   poetry env use python3.11
   poetry install
   ```

5. **Activate the virtual environment**
   ```bash
   poetry shell
   ```

---

## 🐞 Debugging Errors & Fixes

### ❌ Error 1: `ModuleNotFoundError: No module named 'cloudgoat.core'`

📸 _Screenshot_: `debug12.png` 
<img width="712" height="224" alt="debug12" src="https://github.com/user-attachments/assets/69d1abc8-a127-4b30-8ffe-dd4530ba6e19" />


This occurred because the `cloudgoat.py` script was moved outside its original directory.

**Fix:**  
Move `cloudgoat.py` back to the root of the cloned repository (where the `cloudgoat/` package folder is located).

---

### ❌ Error 2: `FileNotFoundError: No such file or directory: '/home/vboxuser/cloudgoat/cloudgoat/scenarios'`

📸 _Screenshot_: `debug13.png`  
<img width="714" height="359" alt="debug13" src="https://github.com/user-attachments/assets/18d5f7ff-9d5c-478e-acf3-f9a73a8948b3" />

This was due to missing or incorrect relative path usage.

**Fix:**  
Ensure the repo structure is intact and you run `cloudgoat.py` from the correct root directory using:
```bash
poetry run python3 cloudgoat.py
```

---

## ✅ Successful Run

📸 _Screenshot_: `debug14.png`  
<img width="702" height="436" alt="debug14" src="https://github.com/user-attachments/assets/9c6cf53b-618b-44ea-9809-e361e200cc07" />

Command ran successfully showing CloudGoat's CLI usage help:

```bash
CloudGoat - https://github.com/RhinoSecurityLabs/cloudgoat

Command info:
  config aws|azure|whitelist|argcomplete [list]
  create <scenario>
  destroy <scenario>|all
  list <scenario>|all|azure|aws
  help <scenario>|<command>
```

---

## 🗂️ Directory Structure (Working State)

```bash
cloudgoat/
├── cloudgoat/         # Python package
│   ├── core/
│   ├── scenarios/
│   └── ...
├── cloudgoat.py       # Main CLI
├── pyproject.toml
├── poetry.lock
└── README.md
```

---

## 💡 Tips

- Always keep `cloudgoat.py` at the same level as the `cloudgoat/` directory.
- Run all commands from the project root.
- Use `poetry run` to avoid issues with environment isolation.
