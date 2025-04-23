## 🔁 Module 5: Control Logic with Loops and Conditions

This module introduces how to use loops and conditionals in Ansible to simplify repetitive tasks and create platform-specific configurations.

### Objectives:
- Use the `loop` directive to iterate over lists of configuration items (e.g., VLANs, address objects).
- Use the `when` directive to apply tasks conditionally based on the platform (e.g., only to Cisco or Palo Alto).
- Build DRY (Don't Repeat Yourself) playbooks that adapt logic to the target device type.

### Live Demo Steps:
> **Note:** Run all commands from inside the `M5-Loops_and_Conditionals/` directory. Use `-i ../M1-Install_Ansible_and_Understand_Inventory/inventory.ini` when running Ansible.

1. Define your variables in `group_vars`:
   ```bash
   mkdir -p group_vars
   touch group_vars/cisco.yml
   touch group_vars/paloalto.yml
   ```

   Example `group_vars/cisco.yml`:
   ```yaml
   vlan_ids:
     - 10
     - 20
     - 30
   ```

   Example `group_vars/paloalto.yml`:
   ```yaml
   address_objects:
     - name: WebServer
       ip: 192.0.2.10
     - name: DBServer
       ip: 192.0.2.20
   ```

2. Create the Cisco VLAN playbook:
   ```bash
   touch configure_vlans.yml
   ```

   Example `configure_vlans.yml`:
   ```yaml
   - name: Configure VLANs on Cisco
     hosts: all
     gather_facts: no
     connection: network_cli

     tasks:
       - name: Configure VLANs
         ios_config:
           lines: "vlan {{ item }}"
         loop: "{{ vlan_ids }}"
         when: ansible_network_os == 'ios'
   ```

3. Create the Palo Alto address objects playbook:
   ```bash
   touch configure_address_objects.yml
   ```

   Example `configure_address_objects.yml`:
   ```yaml
   - name: Configure address objects on Palo Alto
     hosts: all
     gather_facts: no

     vars:
       device:
         ip_address: "{{ ansible_host }}"
         username: "{{ ansible_user }}"
         password: "{{ ansible_password }}"

     tasks:
       - name: Create address objects
         paloaltonetworks.panos.panos_address_object:
           provider: "{{ device }}"
           name: "{{ item.name }}"
           value: "{{ item.ip }}"
         loop: "{{ address_objects }}"
         when: ansible_network_os == 'paloaltonetworks.panos.panos'
   ```

### Running the Playbooks

To execute the playbook from within the `M5-Loops_and_Conditionals/` directory, run:

```bash
ansible-playbook configure_vlans.yml -i ../M1-Install_Ansible_and_Understand_Inventory/inventory.ini
```

Or for Palo Alto:

```bash
ansible-playbook configure_address_objects.yml -i ../M1-Install_Ansible_and_Understand_Inventory/inventory.ini
```

### Optional Exercise:
- Write a playbook that loops through a list of VLANs and applies the configuration only to Cisco devices using a `when` condition.
