# Ansible Onboarding Workspace

This repository contains a production-ready Ansible dev workstation setup following team-friendly standards. It is intended to help new engineers get up and running quickly with a clean, consistent environment.

---

## 1. Environment & Ansible Installation

* **OS:** Ubuntu on WSL2 (Windows 11)
* **Python Version:** 3.11.7
* **Ansible Version:** `ansible --version`
* **Ansible-Lint Version:** `ansible-lint --version`

### Setup Steps:

```bash
# Navigate to project folder
cd ~/ansible-onboarding

# Create and activate isolated Python environment
python3 -m venv .venv
source .venv/bin/activate

# Upgrade pip and install tools
python -m pip install --upgrade pip
python -m pip install ansible ansible-lint yamllint

# Save dependencies
python -m pip freeze > requirements.txt
```

---

## 2. VS Code Setup

### Extensions Installed:

* Red Hat Ansible
* YAML (Red Hat)
* Python
* EditorConfig (optional but recommended)

### Settings (.vscode/settings.json):

```json
{
  "python.defaultInterpreterPath": "${workspaceFolder}/.venv/bin/python",
  "ansible.python.interpreterPath": "${workspaceFolder}/.venv/bin/python",
  "ansibleLint.enabled": true,
  "yaml.validate": true,
  "files.trimTrailingWhitespace": true,
  "editor.formatOnSave": true
}
```

### EditorConfig (.editorconfig):

```
root = true
[*]
charset = utf-8
end_of_line = lf
indent_style = space
indent_size = 2
insert_final_newline = true
trim_trailing_whitespace = true
```

---

## 3. Ansible Configuration

* **File:** `ansible.cfg`
* Team-friendly defaults for inventory, roles, SSH, forks, timeout, and output formatting.
* **Inventory placeholder:** `inventory/hosts.ini`

---

## 4. SSH Readiness

* **SSH Key Location:** `~/.ssh/id_ed25519`
* **SSH Config (~/.ssh/config):**

```
Host *
  ServerAliveInterval 30
  ServerAliveCountMax 4
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_ed25519
  StrictHostKeyChecking ask
```

### Commands to load key:

```bash
# Start ssh-agent if not running
eval "$(ssh-agent -s)"

# Add your SSH key
ssh-add ~/.ssh/id_ed25519

# Verify key is loaded
ssh-add -l
```

> **Tip for WSL2:** The SSH agent doesn’t persist automatically across terminal sessions. Add the following to `~/.bashrc` for automatic startup:

```bash
# Start ssh-agent if not running
if ! pgrep -u "$USER" ssh-agent > /dev/null; then
    eval "$(ssh-agent -s)"
fi

# Add SSH key if not loaded
ssh-add -l &>/dev/null || ssh-add ~/.ssh/id_ed25519
```

---

## 5. Git Identity & Pre-Commit Hooks

```bash
# Configure git identity
git config --global user.name "Your Name"
git config --global user.email "your.email@company.com"
git config --global init.defaultBranch main

# Optional: enable commit signing
# git config --global commit.gpgsign true

# Install pre-commit
python -m pip install pre-commit
pre-commit install
```

### Pre-commit config (`.pre-commit-config.yaml`):

```yaml
repos:
  - repo: https://github.com/adrienverge/yamllint
    rev: v1.35.1
    hooks:
      - id: yamllint
  - repo: https://github.com/ansible/ansible-lint
    rev: v24.6.1
    hooks:
      - id: ansible-lint
```

---

## 6. New Machine? Do This (Checklist)

1. Clone the repository:

```bash
git clone <repo-url>
cd ansible-onboarding
```

2. Create & activate Python virtual environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

3. Install dependencies:

```bash
python -m pip install -r requirements.txt
```

4. Install VS Code extensions listed above.

5. Add SSH key to agent:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
ssh-add -l
```

6. Install and enable pre-commit hooks:

```bash
pre-commit install
```

7. Verify Ansible and Ansible-lint:

```bash
ansible --version
ansible-lint --version
```

8. Run pre-commit smoke test:

```bash
pre-commit run --all-files
```

9. Configure git signing if required.

10. Check corporate/proxy/CACert notes if applicable.

---

## 7. Reflection

* **Team-Friendly Feature:** Isolated Python environment ensures no system-wide pollution and reproducible installs.
* **Pitfall Avoided:** Avoided global pip installs, which can break other projects.
* **Corporate Notes:** WSL2 Ubuntu used for Windows compatibility; SSH keys and config prepared for secure enterprise usage.

---

## 8. Repo Structure

```
ansible-onboarding/
├── .venv/                  # Python virtual environment
├── .vscode/                # VS Code settings
├── .editorconfig           # Editor configuration
├── ansible.cfg             # Baseline Ansible config
├── inventory/              # Inventory folder with hosts.ini
├── requirements.txt        # Python dependencies
├── .pre-commit-config.yaml # Pre-commit hooks
├── README.md               # This file
└── test.yml                # Example playbook
```


Do you want me to do that?
