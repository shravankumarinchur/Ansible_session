
---

# 🔐 Ansible Vault: Full Example Using `vault.pass` File

## 🧩 Goal

We will:

1. Create a vault password file.
2. Encrypt a secrets file.
3. Use the secrets in a playbook.
4. Run the playbook **without being prompted** by passing the password file.

---

## ✅ Step 1: Create Vault Password File

Create a secure password file to be used with vault:

```bash
openssl rand -base64 2048 > vault.pass
```

🔐 This file contains a **random vault password** that will be used to encrypt and decrypt secrets.


---

## ✅ Step 2: Create Vault-Encrypted Secrets File

Use `ansible-vault` to create the encrypted file **using the password file**:

```bash
ansible-vault create secrets.yml --vault-password-file vault.pass
```

When the editor opens (usually `vi`), add your secrets:

```yaml
db_user: admin
db_password: SuperSecure123!
```

🔒 Now, `secrets.yml` is encrypted and only usable with the password in `vault.pass`.

---

## ✅ Step 3: Run Playbook Using Vault

### 📁 Project Directory Structure

```
vault-demo/
├── inventory/
│   └── hosts.ini
├── secrets.yml          # Encrypted secrets file
├── vault.pass           # Vault password file (ignored in Git)
└── playbook.yml
```

---

### 📄 `inventory/hosts.ini`

```ini
[dbservers]
db1 ansible_host=192.168.1.20
```

---

### 📄 `playbook.yml`

```yaml
- name: Demo using Ansible Vault
  hosts: dbservers
  become: true

  vars_files:
    - secrets.yml

  tasks:
    - name: Show DB credentials (for demo only)
      ansible.builtin.debug:
        msg: "DB User: {{ db_user }}, Password: {{ db_password }}"

    - name: Create DB config file
      ansible.builtin.copy:
        dest: /tmp/db.conf
        content: |
          [database]
          user = {{ db_user }}
          password = {{ db_password }}
        mode: '0600'
```

---

### ▶️ Run the Playbook Without Prompt

```bash
ansible-playbook -i inventory/hosts.ini playbook.yml --vault-password-file vault.pass
```

✅ This will **automatically use the password** from `vault.pass` and execute the playbook using the encrypted variables.

---


## 🧪 Bonus: View or Edit Vault File (with password file)

```bash
ansible-vault view secrets.yml --vault-password-file vault.pass
ansible-vault edit secrets.yml --vault-password-file vault.pass
```

---

## ✅ Summary Table

| Step            | Command                                                                       |
| --------------- | ----------------------------------------------------------------------------- |
| Create password | `openssl rand -base64 2048 > vault.pass`                                      |
| Create secrets  | `ansible-vault create secrets.yml --vault-password-file vault.pass`           |
| Run playbook    | `ansible-playbook -i inventory playbook.yml --vault-password-file vault.pass` |

---


