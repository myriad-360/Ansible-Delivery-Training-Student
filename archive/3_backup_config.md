# Step 3: Back Up a Cisco Device Configuration

## 🧪 Objective
Use a playbook to back up the running config of a Cisco device.

## 📜 Code to Execute

Create a file called `backup_config.yml`:

```yaml
- name: Backup Cisco running config
  hosts: routers
  gather_facts: no
  connection: network_cli
  tasks:
    - name: Get running-config
      ios_command:
        commands: ['show running-config']
      register: config_output

    - name: Save config to file
      copy:
        content: "{{ config_output.stdout[0] }}"
        dest: "backups/{{ inventory_hostname }}-running-config.txt"
```

Run it:

```bash
ansible-playbook backup_config.yml
```

## 🧠 What This Does
- Runs `show running-config` on each device
- Stores the output in a per-device backup file

## 🧩 Why This Matters
Configuration backups are essential for disaster recovery, auditing, and change management.