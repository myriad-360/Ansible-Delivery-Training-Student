


## 🧱 Module 9: Explore Extra Tooling

This module focuses on extending Ansible’s capabilities using collections and the `ansible-doc` utility.

### 📌 Key Concepts

- **Reusable Frameworks**: Collections are modular bundles of roles, plugins, and modules developed by vendors and the community.
- **Galaxy**: Ansible Galaxy is the primary source to search, download, and install these collections.
- **Documentation Access**: `ansible-doc` helps explore how to use each module effectively.

### 🛠️ Live Demo Instructions

1. **Search for a Collection**:  
   > Use the [Ansible Galaxy website](https://galaxy.ansible.com) to search for collections manually.

2. **Install a Collection**:
   ```bash
   ansible-galaxy collection install community.general
   ```
   This command downloads and installs the `community.general` collection, which contains a wide variety of general-purpose modules and plugins.

3. **Explore a Module with `ansible-doc`**:
   ```bash
   ansible-doc community.general.json_query
   ```
   Use this to inspect module documentation, usage examples, and available parameters.

### 🧪 Optional Exercise

Try installing a different collection from Galaxy and test one of its modules on a device in your lab environment. For example:

```bash
ansible-galaxy collection install junipernetworks.junos
ansible-doc junipernetworks.junos.junos_interface
```

Then write a quick playbook to test it against your Juniper device (if available).