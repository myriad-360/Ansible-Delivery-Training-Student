## 🔁 Module 5: Control Logic with Loops and Conditions

This module introduces how to use **loops** and **conditionals** in Ansible to simplify repetitive tasks and build platform-aware automation.

### Objectives:
- Use the `loop` directive to iterate over configuration items (e.g., loopback interfaces, address objects).
- Use the `when` directive to apply tasks conditionally based on platform.
- Build DRY (Don't Repeat Yourself) playbooks that adapt logic to each device type.

---

### Live Demo Steps
> **Note:** Run all commands from inside the `M5-Loops_and_Conditionals/` directory. Use `-i ../M1-Install_Ansible_and_Understand_Inventory/inventory.ini` when running Ansible.

---

### 1. Define Your Variables in `group_vars`

```bash
mkdir -p group_vars
touch group_vars/cisco.yml
touch group_vars/paloalto.yml
```

Example `group_vars/cisco.yml`:

```yaml
loopbacks:
  - id: 1
    ip: 192.0.2.1
  - id: 2
    ip: 192.0.2.2
  - id: 3
    ip: 192.0.2.3
  - id: 4
    ip: 192.0.2.4
```

Example `group_vars/paloalto.yml`:

```yaml
address_objects:
  - name: WebServer
    ip: 192.0.2.10
  - name: DBServer
    ip: 192.0.2.20
```

---

### 2. Create the Cisco Loopback Interfaces Playbook

```bash
touch configure_even_loopbacks.yml
```

Example `configure_even_loopbacks.yml`:

```yaml
- name: Configure even-numbered loopbacks on Cisco IOS-XE
  hosts: cisco
  gather_facts: no
  connection: network_cli

  tasks:
    - name: Configure loopbacks
      cisco.ios.ios_config:
        lines:
          - interface Loopback{{ item.id }}
          - ip address {{ item.ip }} 255.255.255.255
      loop: "{{ loopbacks }}"
      when: (item.id | int) is divisibleby 2
```

---

### 3. Create the Palo Alto Address Objects Playbook

```bash
touch configure_address_objects.yml
```

Example `configure_address_objects.yml`:

```yaml
- name: Configure address objects on Palo Alto devices
  hosts: paloalto
  gather_facts: no

  vars:
    provider:
      ip_address: "{{ ansible_host }}"
      username: "{{ ansible_user }}"
      password: "{{ ansible_password }}"

  tasks:
    - name: Show address objects defined in group_vars
      debug:
        var: address_objects

    - name: Ensure address objects are configured
      paloaltonetworks.panos.panos_address_object:
        provider: "{{ provider }}"
        name: "{{ item.name }}"
        value: "{{ item.ip }}"
        description: "Configured via Ansible"
        address_type: "ip-netmask"
      loop: "{{ address_objects }}"
```

---

### 🔧 Running the Playbooks

Run the Cisco playbook:

```bash
ansible-playbook configure_even_loopbacks.yml -i ../M1-Install_Ansible_and_Understand_Inventory/inventory.ini
```

Run the Palo Alto playbook:

```bash
ansible-playbook configure_address_objects.yml -i ../M1-Install_Ansible_and_Understand_Inventory/inventory.ini
```

---

### 🔎 Validation (Cisco)

SSH into the router:

```bash
ssh labuser@192.168.232.253
```

Check loopbacks:

```bash
show ip interface brief | include Loopback
```

Or get full config:

```bash
show running-config | section interface Loopback
```

---

### Optional Exercise

Write a teardown playbook that removes loopback interfaces or address objects by looping over the same variables and setting the `state` to `absent` where supported.
