## 🧪 Module 4: Variables and Templates

This module introduces how to use variables and templates to create dynamic, reusable configuration across different device types.

### Objectives:
- Parameterize Ansible playbooks using `group_vars`.
- Use Jinja2 templates to generate dynamic configuration.
- Render banners, interfaces, or NTP configurations based on device-specific data.

### Live Demo Steps:
> **Note:** Run all commands from inside the `M4-Variables_and_Templates/` directory. The `group_vars/` and `templates/` folders are located here. Use `-i ../M1-Install_Ansible_and_Understand_Inventory/inventory.ini` when running Ansible.
1. Create `group_vars` directories and files for Cisco and Palo Alto groups:
   ```bash
   mkdir -p group_vars
   touch group_vars/cisco.yml
   touch group_vars/paloalto.yml
   ```

2. Add device-specific variables to each file:

   `M4-Variables_and_Templates/group_vars/cisco.yml`
   ```yaml
   ntp_servers:
     - 192.0.2.1
     - 192.0.2.2
   ```

   `M4-Variables_and_Templates/group_vars/paloalto.yml`
   ```yaml
   ntp_servers:
     - 192.0.2.1
     - 192.0.2.2
   ```

3. Create a Jinja2 template for NTP configuration:

   ```bash
   mkdir -p templates
   touch templates/ntp_config.j2
   ```

   Example `M4-Variables_and_Templates/templates/ntp_config.j2` (Cisco-style):
   ```jinja2
   {% for server in ntp_servers %}
   ntp server {{ server }}
   {% endfor %}
   ```

   Example `M4-Variables_and_Templates/templates/ntp_config_palo.xml.j2` (Palo Alto-style):
   ```jinja2
   <ntp-servers>
     {% if ntp_servers|length > 0 %}
     <primary-ntp-server>
       <ntp-server-address>{{ ntp_servers[0] }}</ntp-server-address>
     </primary-ntp-server>
     {% endif %}
     {% if ntp_servers|length > 1 %}
     <secondary-ntp-server>
       <ntp-server-address>{{ ntp_servers[1] }}</ntp-server-address>
     </secondary-ntp-server>
     {% endif %}
   </ntp-servers>
   ```

4. Create a playbook to render or apply NTP configuration:
   ```bash
   touch M4-Variables_and_Templates/apply_ntp_config.yml
   ```

   Example `apply_ntp_config.yml`:
   ```yaml
   - name: Deploy NTP configuration using templates
     hosts: all
     gather_facts: no
     vars:
       device:
         ip_address: "{{ ansible_host }}"
         username: "{{ ansible_user }}"
         password: "{{ ansible_password }}"

     tasks:
       - name: Render Cisco NTP config
         template:
           src: templates/ntp_config.j2
           dest: /tmp/ntp.cfg
         when: ansible_network_os == 'ios'

       - name: Apply Palo Alto NTP config
         paloaltonetworks.panos.panos_config_element:
           provider: "{{ device }}"
           xpath: "/config/devices/entry[@name='localhost.localdomain']/deviceconfig/system"
           element: "{{ lookup('template', 'templates/ntp_config_palo.xml.j2') }}"
         when: ansible_network_os == 'paloaltonetworks.panos.panos'
   ```

### Running the Playbook

To execute the playbook from within the `M4-Variables_and_Templates/` directory, run:

```bash
ansible-playbook apply_ntp_config.yml -i ../M1-Install_Ansible_and_Understand_Inventory/inventory.ini
```

This will apply the rendered NTP configuration to all devices in the inventory using the templates and group variables defined in this module.

### Optional Exercise:
- Write a banner template using Jinja2.
- Apply the banner to both Cisco and Palo Alto devices using device-specific variables.