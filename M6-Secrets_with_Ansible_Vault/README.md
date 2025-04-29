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

Encrypt the entire file instead of individual variables, you can do so with:

```bash
ansible-vault encrypt group_vars/paloalto.yml
```

This will encrypt the full contents of the file, making it unreadable without the vault password. This approach is often used when all contents of a file are sensitive, but it can make version control harder to manage since diffs and merges are no longer human-readable.

---

### 3. Reference Vault Variables in Your Playbook

Here is a full playbook example showing how to gather high-level facts from both Cisco and Palo Alto devices using Vault-managed credentials:

Create the file `gather_device_facts_with_vault.yml` with the contents below
```yaml
- name: Gather high-level facts from Cisco and Palo Alto devices using Vault
  hosts: all
  gather_facts: no

  vars_files:
    - vault.yml

  vars:
    device:
      ip_address: "{{ ansible_host }}"
      username: "{{ ansible_user }}"
      password: "{{ ansible_password }}"

  tasks:
    - name: Gather Cisco facts
      ios_facts:
      when: ansible_network_os == 'ios'

    - name: Print Cisco hostname
      debug:
        var: ansible_net_hostname
      when: ansible_network_os == 'ios'

    - name: Gather Palo Alto facts
      paloaltonetworks.panos.panos_facts:
        provider: "{{ device }}"
      when: ansible_network_os == 'paloaltonetworks.panos.panos'

    - name: Display Palo Alto facts (hostname)
      debug:
        msg: |
          Hostname: "{{ ansible_facts['net_hostname'] }}"
      when: ansible_network_os == 'paloaltonetworks.panos.panos'
```

Run the playbook with the vault password prompt to decrypt credentials securely:

```bash
ansible-playbook gather_device_facts_with_vault.yml -i inventory-withvault.ini --ask-vault-pass
```

### Important Notes
- This example assumes `ansible_password` is stored in `cisco.yml` and `paloalto.yml` and device-specific variables are defined in `group_vars` files.
- Also, note that we're using the local directory `inventory-withvault.ini` file
- Even though these files are encrypted, it's still advised to avoid committing them to source control


