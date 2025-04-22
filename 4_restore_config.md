# Step 4: Restore a Cisco Configuration from File

## 🧪 Objective
Use Ansible to restore a previously backed-up configuration.

## 📜 Code to Execute

Create `restore_config.yml`:

```yaml
- name: Restore config from file
  hosts: routers
  gather_facts: no
  connection: network_cli
  tasks:
    - name: Load config
      ios_config:
        lines: "{{ lookup('file', 'backups/' + inventory_hostname + '-running-config.txt') | splitlines }}"
```

Run it:

```bash
ansible-playbook restore_config.yml
```

## 🧠 What This Does
- Reads the config from the backup file
- Pushes it line-by-line to the router

## 🧩 Why This Matters
Restoring from backup allows you to quickly recover from misconfigurations or test rollback procedures.