## 🧪 Module 3: Write and Execute Playbooks

This module focuses on writing and executing basic Ansible playbooks that interact with both Cisco and Palo Alto network devices. Students will gain hands-on experience defining tasks, using network modules, applying conditionals, and running Ansible in dry-run mode.

Playbooks allow you to define a set of instructions that Ansible executes in order. Each task runs a module with specific parameters, and logic can be applied to target specific platforms differently based on device type. This module teaches the foundational building blocks for all network automation workflows.

### Concepts Covered:
- Playbook structure (YAML format)
- Tasks and modules
- Conditional logic using `when`
- Running in dry-run mode with `--check`
- Platform-specific modules: `ios_facts`, `panos_facts`

### Objectives:
- Learn how to write and execute Ansible playbooks.
- Use conditionals to differentiate between device types.
- Run tasks in check mode to simulate changes.

### Live Demo Steps:
1. Create a new playbook that applies to all devices (`hosts: all`) and disables fact gathering (`gather_facts: no`).
2. Define device connection variables using `ansible_host`, `ansible_user`, and `ansible_password`.
3. Use the `ios_facts` module to gather information from Cisco devices. Wrap the task in a `when` clause to target only IOS devices.
4. Print the Cisco hostname using the `debug` module, again gated by a `when` clause.
5. Use `panos_facts` to gather data from Palo Alto devices by passing in the device connection dictionary.
6. Display key facts from the Palo Alto device (e.g., hostname, serial number, software version) using `debug`.

### Example Playbook:

Create a new file `show_config.yml`
```yaml
---
- name: Gather high-level facts from Cisco and Palo Alto devices
  hosts: all
  gather_facts: no

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

    - name: Display Palo Alto facts (hostname, serial, version)
      debug:
        msg: |
          "Hostname: {{ ansible_facts['net_hostname'] }}"
      when: ansible_network_os == 'paloaltonetworks.panos.panos'
```

### Running the Playbook

To perform a dry run (no changes are made, but you can preview what would happen):

```bash
ansible-playbook -i ../M1-Install_Ansible_and_Understand_Inventory/inventory.ini show_facts.yml --check
```

To execute the playbook against your inventory, use the `ansible-playbook` command:

```bash
ansible-playbook -i ../M1-Install_Ansible_and_Understand_Inventory/inventory.ini show_facts.yml
```

### Optional Exercise:
- Expand the playbook to retrieve additional details from the Palo Alto firewall such as the software version, serial number, or interface information. Use the `debug` module to print each value clearly.