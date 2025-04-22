# Step 1: Configure a Hostname on a Cisco Device

## 🧪 Objective
Use an Ansible playbook to set the hostname on a Cisco router.

## 📜 Code to Execute

Create a file called `set_hostname.yml` and paste in the following:

```yaml
- name: Configure hostname
  hosts: routers
  gather_facts: no
  connection: network_cli
  tasks:
    - name: Set hostname
      ios_config:
        lines:
          - hostname R1
```

Then run the playbook:

```bash
ansible-playbook set_hostname.yml
```

## 🧠 What This Does
- Connects to devices in the `routers` group
- Uses CLI over SSH
- Sends the command `hostname R1` to the device

## 🧩 Why This Matters
Ansible replaces manual SSH sessions. You declare the config, and Ansible applies it.