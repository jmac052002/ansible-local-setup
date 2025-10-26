# 🧱 Ansible Local Server Setup (Flask → Gunicorn → Nginx)

[![Ansible Lint](https://github.com/jmac052002/ansible-local-setup/actions/workflows/lint.yml/badge.svg)](https://github.com/jmac052002/ansible-local-setup/actions)
[![Made with Ansible](https://img.shields.io/badge/Made%20with-Ansible-1A73E8?logo=ansible&logoColor=white)](https://www.ansible.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

This project automates provisioning of a complete **local web stack** using **Ansible**:
- Installs system essentials
- Sets timezone and optional UFW
- Installs and configures **Docker**
- Deploys a **Flask app** behind **Gunicorn** and **Nginx**
- Can run on **localhost**, **WSL**, or a **remote EC2**

---

## 🧩 Project Overview

| Layer | Tool | Purpose |
|-------|------|----------|
| OS | Ubuntu / Debian | Tested environment |
| Config Mgmt | Ansible | Idempotent provisioning |
| App | Flask | Simple Python web API |
| WSGI | Gunicorn | Serves Flask over localhost:8000 |
| Reverse Proxy | Nginx | Proxies traffic to Flask |
| Optional | Docker, UFW | Demonstration of modular roles |

---

## 📁 Project Tree

```text
ansible-local-setup/
├── ansible.cfg
├── inventory.ini
├── group_vars/
│   └── all.yml
├── playbooks/
│   └── setup.yml
├── roles/
│   ├── common/
│   │   ├── tasks/main.yml
│   │   ├── handlers/main.yml
│   │   └── templates/motd.j2
│   ├── docker/
│   │   └── tasks/main.yml
│   ├── flask_app/
│   │   ├── tasks/main.yml
│   │   └── templates/
│   │       ├── app.py.j2
│   │       ├── wsgi.py.j2
│   │       └── gunicorn.service.j2
│   └── nginx/
│       ├── tasks/main.yml
│       └── templates/
│           ├── flask_site.j2
│           ├── default_static.j2
│           └── index.html.j2
└── .github/
    └── workflows/lint.yml
