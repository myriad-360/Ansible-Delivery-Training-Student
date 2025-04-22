## ✅ Ad Hoc Ansible Commands to Run

### 🔹 Ping All Devices
```bash
ansible all -i ../M1-Install_Ansible_and_Understand_Inventory/inventory.ini -m ping
```

### 🔹 Gather Cisco Facts
```bash
ansible c8000v -i ../M1-Install_Ansible_and_Understand_Inventory/inventory.ini -m ios_facts -c network_cli
```

### 🔹 Gather Palo Alto Facts
```bash
ansible pa-fw -i ../M1-Install_Ansible_and_Understand_Inventory/inventory.ini -m panos_facts -c httpapi
```

### 🔹 Cisco Command: show version
```bash
ansible c8000v -i ../M1-Install_Ansible_and_Understand_Inventory/inventory.ini -m ios_command -a "commands=['show version']"
```

### 🔹 Palo Alto Command: show system info
```bash
ansible pa-fw -i ../M1-Install_Ansible_and_Understand_Inventory/inventory.ini -m panos_op -a "cmd='show system info'" -c httpapi
```