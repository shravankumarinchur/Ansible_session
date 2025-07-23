

---

# 🔁 Ansible Conditions – Explained with Real Examples

## ✅ What Are Conditions in Ansible?

Ansible **conditions** let you run a task **only when a certain condition is true**.
This is done using the `when:` keyword in a task.

---

## 💡 Why Use `when` Conditions?

You should use `when:` to:

* Skip tasks based on **OS, version, or hostname**
* Act based on **variable values or registered output**
* Avoid failures by checking if files, services, or values exist

---

## ✨ Example 1: Run a Task Only on a Specific OS

📝 Suppose you want to install a package **only if the OS is RedHat**.

### ✅ Playbook: `install_httpd_redhat.yml`

```yaml
- name: Install Apache only on RedHat systems
  hosts: all
  become: true

  tasks:
    - name: Install Apache (httpd)
      ansible.builtin.yum:
        name: httpd
        state: present
      when: ansible_os_family == "RedHat"
```

### 🔍 Explanation:

* `ansible_os_family` is an **Ansible fact**
* The task will run only if the target system is RedHat-based (like CentOS or RHEL)
* This avoids errors on Ubuntu or Debian machines

---



---

## 🔁 Other Common Conditional Use Cases

| Use Case               | Example Condition                        |
| ---------------------- | ---------------------------------------- |
| Variable is defined    | `when: my_var is defined`                |
| OS is Ubuntu           | `when: ansible_distribution == "Ubuntu"` |
| Combine conditions     | `when: x == 1 and y == 2`                |
| Registered task result | `when: result.rc == 0`                   |

---

## ✅ Summary

* Use `when:` to control task execution
* Great for platform-specific tasks, file checks, or logic control
* Works with facts, variables, and registered task output

---
