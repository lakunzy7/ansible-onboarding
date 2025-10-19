Ah! Got it — you want the **README.md formatted so that all commands are immediately copyable by learners**, without extra explanations inside the code blocks. Essentially, each code block should contain **just the commands**, ready to copy-paste. I’ve cleaned it up for that purpose:

````markdown
# Ansible Onboarding Workspace

This repository contains a production-ready Ansible dev workstation setup.

---

## 1. Environment & Ansible Installation

Check versions:
```bash
ansible --version
ansible-lint --version
````

Setup environment:

```bash
cd ~/ansible-onboarding
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install ansible ansible-lint yamllint
python -m pip freeze > requirements.txt
```

---

## 2. VS Code Setup

Settings `.vscode/settings.json`:

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

EditorConfig `.editorconfig`:

```ini
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

## 3. SSH Readiness

SSH commands:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
ssh-add -l
```

SSH config `~/.ssh/config`:

```text
Host *
  ServerAliveInterval 30
  ServerAliveCountMax 4
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_ed25519
  StrictHostKeyChecking ask
```

---

## 4. Git & Pre-Commit Hooks

Git setup:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@company.com"
git config --global init.defaultBranch main
```

Optional commit signing:

```bash
# git config --global commit.gpgsign true
```

Pre-commit installation:

```bash
python -m pip install pre-commit
pre-commit install
```

Pre-commit run:

```bash
pre-commit run --all-files
```

Pre-commit config `.pre-commit-config.yaml`:

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

## 5. New Machine Setup Checklist

```bash
git clone <repo-url>
cd ansible-onboarding
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
ssh-add ~/.ssh/id_ed25519
ssh-add -l
pre-commit install
ansible --version
ansible-lint --version
pre-commit run --all-files
```

---

## 6. Repo Structure

```text
ansible-onboarding/
├── .venv/
├── .vscode/
├── .editorconfig
├── ansible.cfg
├── inventory/
├── requirements.txt
├── .pre-commit-config.yaml
├── README.md
└── test.yml
```

```
