# NSCC Ansible Lab Env
https://redmondo.notion.site/NETW3500-Ansible-Automation-b7bb7bb062454cb0bf4f7d636c32ae4f

## Overview
This lab provides a containerized environment to learn Ansible. You will use your host machine (Windows/Mac/Linux) to run three Ubuntu containers as managed nodes, and your WSL2 or Linux environment as the **Ansible Control Node**.

---

## Setup Instructions

### 1. Prerequisites
- **Docker Desktop:** Ensure it is running and WSL2 integration is enabled.
- **Ansible (Control Node):** Install Ansible inside your **WSL (Ubuntu)** terminal:
  ```bash
  sudo apt update && sudo apt install ansible -y
  ```

### 2. Clone the Repository (CRITICAL)
To avoid Windows/Linux permission issues (like "Permissions 0777 are too open"), **you must clone this repository directly into your WSL home directory**, NOT your Windows C: drive.

In your WSL terminal, run:
```bash
cd ~
git clone <this-repo-url>
cd ansible_lab
```

### 3. Spin Up Lab Environment
From the `ansible_lab` folder in WSL:
```bash
docker-compose up -d
```

### 4. Fix SSH Key Permissions
Ansible requires strict permissions on the private key:
```bash
chmod 600 docker/ssh/ubuntu
```

### 5. Verify Connection
Test connectivity to your nodes using the provided inventory:
```bash
ansible all -i inventory.ini -m ping
```

---

## NGINX Demo

The Nginx demo automates the deployment of a static website using Ansible modules (`apt`, `unarchive`, `template`, `file`).

### 1. Run the Playbook
```bash
ansible-playbook -i inventory.ini ansible-nginx-demo/playbook.yml
```

### 2. Verify Deployment
Once the playbook completes, open your browser on your host machine and visit:
- [http://localhost:1080](http://localhost:1080)
- [http://localhost:2080](http://localhost:2080)
- [http://localhost:3080](http://localhost:3080)

---

## Technical Details
- **User:** `itstudent`
- **SSH Key:** `./docker/ssh/ubuntu` (Pre-configured in `inventory.ini`)
- **Sudo:** Pre-configured with NOPASSWD for `itstudent`.
