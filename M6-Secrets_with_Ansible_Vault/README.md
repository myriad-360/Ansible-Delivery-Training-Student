## 🔐 Module 6: Protect Sensitive Info and Manage Inventories

This module introduces how to use **Ansible Vault** to secure secrets like API keys and passwords, and how to integrate those secrets into your inventories and playbooks securely.

### Objectives:
- Use `ansible-vault encrypt_string` to store secrets inline in variable files.
- Encrypt a centralized vault file with sensitive information like login credentials.
- Reference vault-encrypted variables in playbooks and group_vars.
- Use `--ask-vault-pass` to securely prompt for the vault password at runtime.

---

### Live Demo Steps
> **Note:** Run all commands from inside the `M6-Secrets_with_Ansible_Vault/` directory. Use `-i ../M1-Install_Ansible_and_Understand_Inventory/inventory.ini` when running Ansible.

---

### 1. Encrypt a Cisco password using `ansible-vault encrypt_string`

Run this command and provide a password when prompted:

```bash
ansible-vault encrypt_string 'livestock Taco4' --name 'ansible_password'
```

Copy the resulting encrypted variable block into `group_vars/cisco.yml` like this:

```yaml
ansible_user: labuser
ansible_password: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          # (vault text for 'livestock Taco4' will appear here)
          ...
```

---

### 2. Encrypt a Palo Alto password and store it in `group_vars/paloalto.yml`

Repeat the process used for Cisco, this time encrypting the Palo Alto login password.

Run this command and set the same vault password when prompted:

```bash
ansible-vault encrypt_string 'livestock Taco4' --name 'ansible_password'
```

Create or edit the file `group_vars/paloalto.yml` and paste in the encrypted output:

```yaml
ansible_user: labuser
ansible_password: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          # (vault text for 'livestock Taco4' will appear here)
          ...
```

This ensures the Palo Alto host group has its own securely encrypted credentials scoped to its group.

If you'd prefer to encrypt the entire file instead of individual variables, you can do so with:

```bash
ansible-vault encrypt group_vars/paloalto.yml
```

This will encrypt the full contents of the file, making it unreadable without the vault password. This approach is often used when all contents of a file are sensitive, but it can make version control harder to manage since diffs and merges are no longer human-readable.

---

### 3. Reference Vault Variables in Your Playbook

In your Palo Alto playbook (e.g., `configure_address_objects.yml`), load the encrypted vault with `vars_files`:

```yaml
vars_files:
  - vault.yml

vars:
  provider:
    ip_address: "{{ ansible_host }}"
    username: "admin"
    api_key: "{{ palo_api_key }}"
```

Make sure the playbook is executed with the vault password prompt:

```bash
ansible-playbook configure_address_objects.yml -i ../M1-Install_Ansible_and_Understand_Inventory/inventory.ini --ask-vault-pass
```

---

### Optional Exercise

Create a `vault.yml` file that contains both the Cisco and Palo Alto credentials:

```yaml
ansible_password: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          # (vault text for 'livestock Taco4' will appear here)
          ...

palo_api_key: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          # (vault text for 'livestock Taco4' will appear here)
          ...
```

To create this centralized vault file, you can start by creating a plain `vault.yml` file with the variables, then encrypt it using:

```bash
ansible-vault encrypt vault.yml
```

This command will encrypt the entire `vault.yml` file, securing all contained secrets at once.

Then, refactor your playbooks to consume these variables from a single encrypted file using `vars_files`.