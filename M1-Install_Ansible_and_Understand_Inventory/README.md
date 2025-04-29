## ✅ Ansible Inventory Commands (Module 1)

# TODO: Add instructions for connecting via ssh (either via terminal or vs code)
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

### 🔹 Test a Single Host’s Variables
```bash
ansible-inventory -i inventory.ini --host c8000v
ansible-inventory -i inventory.ini --host pa-fw
```