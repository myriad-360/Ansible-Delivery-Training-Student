


## 🧱 Module 9: Explore Extra Tooling

This module focuses on extending Ansible’s capabilities using collections and the `ansible-doc` utility.

### 📌 Key Concepts

- **Reusable Frameworks**: Collections are modular bundles of roles, plugins, and modules developed by vendors and the community.
- **Galaxy**: Ansible Galaxy is the primary source to search, download, and install these collections.
- **Documentation Access**: `ansible-doc` helps explore how to use each module effectively.

### 🛠️ Live Demo Instructions

1. **Search for a Collection**:
   ```bash
   ansible-galaxy collection search panos
   ```
   This command queries Ansible Galaxy for collections related to Palo Alto firewalls.

2. **Install a Collection**:
   ```bash
   ansible-galaxy collection install paloaltonetworks.panos
   ansible-galaxy collection install cisco.ios
   ```
   These commands download and install the required collections for Palo Alto and Cisco device management.

3. **Explore a Module with `ansible-doc`**:
   ```bash
   ansible-doc paloaltonetworks.panos.panos_address_object
   ansible-doc cisco.ios.ios_config
   ```
   Use this to inspect module documentation, usage examples, and available parameters.

### 🧪 Optional Exercise

Try installing a different collection from Galaxy and test one of its modules on a device in your lab environment. For example:

```bash
ansible-galaxy collection install junipernetworks.junos
ansible-doc junipernetworks.junos.junos_interface
```

Then write a quick playbook to test it against your Juniper device (if available).