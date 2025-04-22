## ✅ Ansible Inventory Commands (Module 1)

### 🔹 Display INI Inventory as JSON
```bash
ansible-inventory -i inventory.ini --list
```

### 🔹 Display INI Inventory in Host List Format
```bash
ansible-inventory -i inventory.ini --graph
```

### 🔹 Verify Inventory Syntax (YAML Output)
```bash
ansible-inventory -i inventory.ini --list --yaml
```

### 🔹 Show Group Hierarchy
```bash
ansible-inventory -i inventory.ini --graph
```

### 🔹 Test a Single Host’s Variables
```bash
ansible-inventory -i inventory.ini --host c8000v
ansible-inventory -i inventory.ini --host pa-fw
```