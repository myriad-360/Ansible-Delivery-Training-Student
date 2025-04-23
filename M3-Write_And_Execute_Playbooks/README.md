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
- name: Gather high-level facts from Cisco and Palo Alto devices
  hosts: all
  gather_facts: no

  tasks:
    - name: Gather Cisco facts
      ios_facts:
      when: ansible_network_os == 'ios'

    - name: Gather Palo Alto facts
      paloaltonetworks.panos.panos_facts:
        provider:
          ip_address: "{{ ansible_host }}"
          username: "{{ ansible_user }}"
          password: "{{ ansible_password }}"
      when: ansible_network_os == 'paloaltonetworks.panos.panos'
```

### Optional Exercise:
- Modify the playbook to set different hostnames per group using `group_vars`.
- Add a `debug` task to confirm the hostname value per device before execution.