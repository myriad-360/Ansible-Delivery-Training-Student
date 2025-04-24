## 🧱 Module 8: Build Reusable Automation Frameworks

This module introduces how to create reusable, modular roles in Ansible to support scalable, version-controlled automation across different platforms.

### Objectives:
- Use `ansible-galaxy init` to scaffold a role structure.
- Create vendor-specific roles (Cisco and Palo Alto).
- Understand how roles support reuse, collaboration, and scaling.

---

### Live Demo Steps

> **Note:** Run these steps from inside the `M8-Reusable_Frameworks/` directory. Use the inventory file from Module 6: `inventory-withvault.ini`.

---

### 1. Scaffold Roles for Each Vendor

Use the `ansible-galaxy` command to create a skeleton role for each vendor:

```bash
mkdir roles
ansible-galaxy init roles/cisco_base
ansible-galaxy init roles/palo_base
```

---

### 2. Write a Basic Task in Each Role

**Edit `roles/cisco_base/tasks/main.yml`:**

```yaml
- name: Show version on Cisco device
  ios_command:
    commands: show version
  register: cisco_version

- name: Display Cisco version output
  debug:
    var: cisco_version.stdout_lines
```

**Edit `roles/palo_base/tasks/main.yml`:**

```yaml
- name: Retrieve system info from Palo Alto
  paloaltonetworks.panos.panos_op:
    provider:
      ip_address: "{{ ansible_host }}"
      username: "{{ ansible_user }}"
      password: "{{ ansible_password }}"
    cmd: "show system info"
  register: palo_info

- name: Display Palo Alto system info
  debug:
    var: palo_info.stdout_lines
```

---

### 3. Create the Role-Based Playbook

Create a new playbook file:

```bash
touch run_roles.yml
```

**`run_roles.yml`:**

```yaml
- name: Run platform-specific base roles
  hosts: all
  gather_facts: no

  pre_tasks:
    - name: Set provider for Palo Alto
      set_fact:
        provider:
          ip_address: "{{ ansible_host }}"
          username: "{{ ansible_user }}"
          password: "{{ ansible_password }}"
      when: ansible_network_os == 'paloaltonetworks.panos.panos'

  roles:
    - { role: cisco_base, when: ansible_network_os == 'ios' }
    - { role: palo_base, when: ansible_network_os == 'paloaltonetworks.panos.panos' }
```

---

### 4. Run the Playbook

```bash
ansible-playbook run_roles.yml -i ../M6-Secrets_with_Ansible_Vault/inventory-withvault.ini --ask-vault-pass
```

---

This modular approach encourages reuse, easier collaboration, and supports version control of logic per platform.
