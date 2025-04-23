## 🧠 Understanding Ad Hoc Command Structure

- `all`: Refers to all hosts listed in the inventory.
- `cisco`: Refers to the host group named `[cisco]` in the inventory file.
- `-i`: Specifies the path to the inventory file.
- `-m`: Specifies the module to run (e.g., `ping`, `ios_facts`).
- `-a`: Specifies the arguments to pass to the module (e.g., command input, configuration lines).

These components work together to let you target specific devices and run specific Ansible modules without needing a full playbook.

## ✅ Ad Hoc Ansible Commands to Run

### 🔹 Ping All Devices
```bash
ansible all -i ../M1-Install_Ansible_and_Understand_Inventory/inventory.ini -m ping
```

### 🔹 Run ios_facts on the Cisco Router
```bash
ansible cisco -i ../M1-Install_Ansible_and_Understand_Inventory/inventory.ini -m ios_facts
```