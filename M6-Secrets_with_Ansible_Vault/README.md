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

### 2. Encrypt a Palo Alto API key and store it in `vault.yml`

Run this command (using the same password for now as a placeholder):

```bash
ansible-vault encrypt_string 'livestock Taco4' --name 'palo_api_key'
```

Create a new file called `vault.yml` and paste the encrypted key:

```yaml
palo_api_key: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          # (vault text for 'livestock Taco4' will appear here)
          ...
```

Then encrypt the entire file (if desired):

```bash
ansible-vault encrypt vault.yml
```

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

Then, refactor your playbooks to consume these variables from a single encrypted file using `vars_files`.