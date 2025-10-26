
# Ansible: Local Server Setup Automation (Starter Pack)

This project automates the initial setup of a Linux machine (tested on Ubuntu/Debian) and can run **locally** (localhost) or against remote hosts over SSH. It installs common packages, sets the timezone, installs Docker, and configures Nginx with a simple web page.

## What it does
- Updates package cache and upgrades packages (safe mode)
- Installs essentials: `git`, `curl`, `htop`, `unzip`
- Sets the timezone (default: `America/Chicago`)
- Installs and enables **Docker**
- Installs and configures **Nginx** with a basic landing page
- (Optional) Enables UFW and allows HTTP/SSH (disabled by default, see vars)

## Quick start (localhost)

> **Prereqs** (on your local machine):  
> - Python 3.8+  
> - Ansible 2.15+ (`pipx install ansible` or `pip install ansible`)  
> - Ubuntu/Debian recommended for this starter

```bash
# 1) cd into the project
cd ansible-local-setup

# 2) Check Ansible can reach localhost
ansible -i inventory.ini all -m ping

# 3) Dry-run (diff/verbose optional)
ansible-playbook -i inventory.ini playbooks/setup.yml --check -v

# 4) Apply
ansible-playbook -i inventory.ini playbooks/setup.yml -v
```

Open the test site:
```bash
# Nginx default page served locally
xdg-open http://localhost || true
```

## Adapting to remote hosts
Edit `inventory.ini` and replace the `local` group with your hosts:
```ini
[web]
1.2.3.4 ansible_user=ubuntu
5.6.7.8 ansible_user=ec2-user

# Optionally, set a key file:
# ansible_ssh_private_key_file=~/.ssh/id_rsa
```
Then run the same `ansible-playbook` command with `-i inventory.ini`.

## Variables
Global variables live in `group_vars/all.yml`:
- `timezone`: e.g. `America/Chicago`
- `enable_ufw`: `false` by default (set to `true` to enable firewall)
- `ufw_allowed_ports`: ports to allow when UFW enabled
- `docker_user`: user to add to the `docker` group (defaults to the Ansible **effective** user if left empty)

## Repo layout
```
ansible.cfg
inventory.ini
group_vars/
  └── all.yml
playbooks/
  └── setup.yml
roles/
  ├── common/
  │   ├── tasks/main.yml
  │   ├── handlers/main.yml
  │   └── templates/motd.j2
  ├── docker/
  │   └── tasks/main.yml
  └── nginx/
      ├── tasks/main.yml
      ├── handlers/main.yml
      └── templates/default.j2
.github/
  └── workflows/lint.yml
```

## CI (optional)
GitHub Actions workflow `lint.yml` runs `ansible-lint` and `yamllint` on PRs.

## Uninstall / rollback
- Docker: `sudo apt-get remove -y docker.io && sudo usermod -G docker -d $(whoami)` (remove manually)
- Nginx: `sudo apt-get purge -y nginx nginx-common`
- MOTD: overwrite `/etc/motd` with your content

> This starter aims to be idempotent and safe to run repeatedly.
