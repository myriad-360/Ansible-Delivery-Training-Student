## 🔁 Module 8: Automate Complex Workflows

This module focuses on building resilient, modular, and vendor-aware workflows in Ansible using features like `include_tasks`, `block`, and `rescue`.

### Objectives:
- Use `include_tasks` to separate logic per vendor.
- Use `block` and `rescue` to gracefully handle errors.
- Route logic dynamically with `when` statements.

---

### Live Demo Steps

> **Note:** Run these steps from inside the `M8-Complex_Workflows/` directory. Use the inventory file from Module 6: `inventory-withvault.ini`.

---

### 1. Set up task files for each vendor

Create two separate task files:

```bash
mkdir -p tasks
touch tasks/cisco_tasks.yml
touch tasks/palo_tasks.yml
```

**tasks/cisco_tasks.yml**
```yaml
- name: Show version on Cisco
  ios_command:
    commands:
      - show version
  register: cisco_output

- name: Print Cisco output
  debug:
    var: cisco_output.stdout_lines
```

**tasks/palo_tasks.yml**
```yaml
- name: Retrieve Palo Alto system info
  paloaltonetworks.panos.panos_op:
    provider: "{{ device }}"
    cmd: "show system info"
  register: palo_output

- name: Print Palo Alto output
  debug:
    var: palo_output.stdout_lines
```

---

### 2. Create the playbook

Create a main playbook to include the vendor-specific task files dynamically:

```bash
touch complex_workflow.yml
```

**complex_workflow.yml**
```yaml
- name: Run complex workflow per vendor
  hosts: all
  gather_facts: no

  vars_files:
    - ../M6-Secrets_with_Ansible_Vault/vault.yml

  vars:
    device:
      ip_address: "{{ ansible_host }}"
      username: "{{ ansible_user }}"
      password: "{{ ansible_password }}"

  tasks:
    - name: Execute Cisco tasks
      include_tasks: tasks/cisco_tasks.yml
      when: ansible_network_os == 'ios'

    - name: Execute Palo tasks in block with simulated failure
      block:
        - name: Simulate a failure
          command: /bin/false
          when: ansible_network_os == 'paloaltonetworks.panos.panos'

        - name: Include Palo tasks
          include_tasks: tasks/palo_tasks.yml
          when: ansible_network_os == 'paloaltonetworks.panos.panos'

      rescue:
        - name: Handle Palo failure gracefully
          debug:
            msg: "Failure caught for Palo Alto. Skipping Palo tasks."
```

---

### 3. Run the workflow

```bash
ansible-playbook complex_workflow.yml -i ../M6-Secrets_with_Ansible_Vault/inventory-withvault.ini --ask-vault-pass
```

This demo will:
- Run Cisco-specific tasks on IOS devices.
- Simulate and recover from a failure on Palo Alto devices.
- Dynamically route logic based on platform using `when`.

---
