
---

# 🔁 Ansible Loops – Explained with Examples

## ✅ What Are Loops in Ansible?

**Loops** let you run a task **multiple times** with different values.
Instead of repeating the same task for each item manually, you can use `loop:` to **iterate** over a list of items.

---

## ✨ Why Use Loops?

* To **install multiple packages**
* To **create multiple users**
* To **copy multiple files**
* To **ensure repeatable actions** on various items with minimal code

---

## ✅ Syntax: Basic Loop

```yaml
- name: Task name
  ansible.builtin.module:
    key: "{{ item }}"
  loop:
    - value1
    - value2
    - value3
```

---

# 📘 Example 1: Install Multiple Packages Using a Loop

### 📄 Playbook: `install_packages.yml`

```yaml
- name: Install required packages
  hosts: all
  become: true

  tasks:
    - name: Install multiple packages using loop
      ansible.builtin.yum:
        name: "{{ item }}"
        state: present
      loop:
        - httpd
        - vim
        - git
```

### 🔍 Explanation

* Installs **3 packages** using the `yum` module
* `item` gets values: `httpd`, `vim`, and `git` one by one

---

# 📘 Example 2: Create Multiple Users with Custom Shells

### 📄 Playbook: `create_users.yml`

```yaml
- name: Create system users
  hosts: all
  become: true

  tasks:
    - name: Add users from a list
      ansible.builtin.user:
        name: "{{ item.name }}"
        shell: "{{ item.shell }}"
      loop:
        - { name: 'alice', shell: '/bin/bash' }
        - { name: 'bob', shell: '/sbin/nologin' }
        - { name: 'eve', shell: '/bin/zsh' }
```

### 🔍 Explanation

* Creates 3 users: `alice`, `bob`, and `eve`
* Each user gets a different shell using **loop with dictionaries**
* `item.name` and `item.shell` reference fields in each dictionary

---

## 🧠 Bonus: Loop Over Files or Templates

You can also loop over files, templates, directories, etc.

```yaml
- name: Copy multiple config files
  ansible.builtin.copy:
    src: "files/{{ item }}"
    dest: "/etc/myapp/{{ item }}"
  loop:
    - config1.ini
    - config2.ini
    - config3.ini
```

---

## ✅ Summary: When to Use Loops

| Use Case         | Good With Modules Like     |
| ---------------- | -------------------------- |
| Install packages | `yum`, `apt`, `dnf`        |
| Create users     | `user`                     |
| Copy files       | `copy`, `template`, `file` |
| Restart services | `service`, `systemd`       |

---
