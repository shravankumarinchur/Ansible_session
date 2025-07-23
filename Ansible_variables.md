
---
# 🧩 Ansible Variables – Simple Guide

### 🔶 What are Variables?
- Variables in Ansible let you store values (like names, paths, IPs) so you don’t repeat them everywhere.

### ✅ Why Use Variables?
 - To avoid repeating values (e.g. server names, ports)
 - To make your playbooks flexible
 - To change behavior based on host/group

## ✅ Example Directory Structure

```
ansible-vars-demo/
├── inventory/
│   └── hosts.ini
├── group_vars/
│   ├── webservers.yml
│   └── dbservers.yml
├── vars/
│   └── external_vars.yml
├── playbooks/
    ├── inline_vars.yml
    ├── external_vars.yml
    ├── command_line_vars.yml
    └── group_vars.yml

```

---

## 🔹 1. `playbooks/inline_vars.yml` — 🟩 Inline Variables

```yaml
- name: Playbook using inline variables
  hosts: all
  become: true

  vars:
    package_name: nginx
    welcome_message: "Installing {{ package_name }} using inline variables"

  tasks:
    - name: Show welcome message
      ansible.builtin.debug:
        msg: "{{ welcome_message }}"

    - name: Install tree package
      ansible.builtin.yum:
        name: "{{ package_name }}"
        state: present
```

> 🎯 **Purpose:** To show how variables can be defined and used directly inside the playbook.

---

## 🔹 2. `playbooks/external_vars.yml` — 📦 External Variable File

```yaml
- name: Playbook using external variable file
  hosts: all
  become: true

  vars_files:
    - ../vars/external_vars.yml

  tasks:
    - name: Show message from external file
      ansible.builtin.debug:
        msg: "{{ message_from_file }}"

    - name: Install package from external var
      ansible.builtin.yum:
        name: "{{ external_package }}"
        state: present
```

### 📄 `vars/external_vars.yml`

```yaml
message_from_file: "This is coming from an external YAML variable file."
external_package: vim
```

> 🎯 **Purpose:** Demonstrate how to cleanly organize variables in a file and reuse them in playbooks.

---

## 🔹 3. `playbooks/command_line_vars.yml` — 🧪 Command-Line Variables

```yaml
- name: Playbook using command-line variables
  hosts: all
  become: true

  tasks:
    - name: Install package from CLI var
      ansible.builtin.yum:
        name: "{{ cli_package }}"
        state: present
```

### ▶️ Run this with:

```bash
ansible-playbook -i inventory/hosts.ini playbooks/command_line_vars.yml -e "cli_package=curl"
```

> 🎯 **Purpose:** Show how variables passed with `-e` override or inject values at runtime.

---

## 🔹 4. `playbooks/group_vars.yml` — 🖥️ Group Variables (Auto-Loaded)

```yaml
- name: Playbook using group_vars
  hosts: all
  become: true

  tasks:
    - name: Print group name and package
      ansible.builtin.debug:
        msg: "Group {{ group_name }} will install {{ group_package }}"

    - name: Install package from group_vars
      ansible.builtin.yum:
        name: "{{ group_package }}"
        state: present
```

### 📄 `group_vars/webservers.yml`

```yaml
group_name: Web Servers
group_package: nginx
```

### 📄 `group_vars/dbservers.yml`

```yaml
group_name: Database Servers
group_package: mariadb-server
```

> 🎯 **Purpose:** Show how group-specific variables are **automatically applied** depending on which group is targeted.

---

## 📄 `inventory/hosts.ini`

```ini
[webservers]
web1 ansible_host=192.168.1.10

[dbservers]
db1 ansible_host=192.168.1.20
```

> 🎯 **Note:** Replace IPs with local or test servers if needed.

---

## ✅ Bonus Tip for Your Session

You can run each playbook like:

```bash
ansible-playbook -i inventory/hosts.ini playbooks/inline_vars.yml
ansible-playbook -i inventory/hosts.ini playbooks/external_vars.yml
ansible-playbook -i inventory/hosts.ini playbooks/command_line_vars.yml -e "custom_message='CLI Hello' cli_package=curl"
ansible-playbook -i inventory/hosts.ini playbooks/group_vars.yml
```

---

