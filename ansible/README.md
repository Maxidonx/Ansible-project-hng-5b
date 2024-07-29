# PHP Laravel Application Setup with Ansible

This repository contains an Ansible playbook for setting up a PHP Laravel application with PostgreSQL on a remote server. The playbook automates the installation and configuration of required software and services, including PostgreSQL, Redis, and Composer, and sets up the Laravel application.

## Table of Contents

- [Overview](#overview)
- [Requirements](#requirements)
- [Setup](#setup)
- [Usage](#usage)
- [Troubleshooting](#troubleshooting)
- [License](#license)

## Overview

This playbook performs the following tasks:
1. Ensures the necessary system users and directories exist.
2. Installs required packages such as PHP, PostgreSQL, Redis, and Composer.
3. Configures PostgreSQL databases and users.
4. Clones the Laravel application repository.
5. Sets up the Laravel application environment.
6. Installs Composer dependencies.

## Requirements

Before running the playbook, ensure you have the following:

- Ansible installed on your local machine.
- Access to a remote server with sudo privileges.
- PostgreSQL, Redis, and PHP repositories available on the server.
- Ansible `community.postgresql` collection installed:

  ```bash
  ansible-galaxy collection install community.postgresql


## Setup

- **Clone this repository:**

```bash
git clone https://your-repository-url.git
cd your-repository-directory
```
- **Configure your inventory file (inventory.cfg):**

```bash
[web]
your_server_ip_or_hostname ansible_user=your_ssh_user
```

- Update the Ansible playbook (main.yaml) with your specific application settings, PostgreSQL credentials, and repository URL.

- **Update ansible.cfg if needed, especially for privilege escalation settings:**

```bash
[defaults]
become = yes
become_method = sudo
become_user = postgres
```

## Usage
Run the Ansible playbook to set up the PHP Laravel application:
```bash
ansible-playbook main.yaml -i inventory.cfg
```
Ensure you have the correct permissions to use sudo without a password.

Troubleshooting
If you encounter issues during execution, consider the following:

- **Permissions Issues:**

Ensure the Ansible user has proper sudo privileges configured. You may need to edit the /etc/sudoers file:

```bash
Copy code
sudo visudo
```
Add:

```bash
your_username ALL=(ALL) NOPASSWD:ALL
Failed Tasks:
```
If you see errors related to PostgreSQL permissions or Composer installation, check the logs for more details. Make sure PostgreSQL and Composer are properly installed and configured.

Debugging:

- **Add debugging tasks to capture more details:**
```
yaml
Copy code
- name: Debug PostgreSQL permissions
  command: ls -l /tmp
  register: result
  become: yes

- name: Show debug output
  debug:
    msg: "{{ result.stdout_lines }}"
```