## 🧪 Module 3: Write and Execute Playbooks

This module introduces writing playbooks for Cisco and Palo Alto network devices.

### Objectives:
- Learn how to write and execute Ansible playbooks.
- Use conditionals to differentiate between device types.
- Run tasks in check mode to simulate changes.

### Live Demo Steps:
1. Write a playbook that sets hostnames for both Cisco and Palo Alto devices.
2. Use `ios_config` for Cisco and `panos_config_element` for Palo Alto.
3. Use `when` statements to apply logic based on `ansible_network_os`.
4. Run the playbook using `--check` to demonstrate dry-run behavior.

### Example Playbook:

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

### Optional Exercise:
- Modify the playbook to get other information from the palo besides the hostname