## 🧱 Module 9: Explore Extra Tooling

This module focuses on extending Ansible’s capabilities using collections and the `ansible-doc` utility.

### 📌 Key Concepts

- **Reusable Frameworks**: Collections are modular bundles of roles, plugins, and modules developed by vendors and the community.
- **Galaxy**: Ansible Galaxy is the primary source to search, download, and install these collections.
- **Documentation Access**: `ansible-doc` helps explore how to use each module effectively.

### 🛠️ Live Demo Instructions

1. **Search for a Collection**:  
   1. Use the [Ansible Galaxy website](https://galaxy.ansible.com), navigate to "Search" on the left hand side bar, and search for "Juniper"
   2. Select "apstra" entry which shows you the "juniper.apstra" collection
   3. Review the install tab
   4. Review the "Documentation" tab
   5. Click on one of the modules listed on the left hand side bar, for example "apstra_facts"
      1. Note the examples section
2. **Install a Collection**:
   ```bash
   ansible-galaxy collection install juniper.apstra
   ```
   This command downloads and installs the `juniper.apstra` collection, which provides a set of Ansible modules and roles for network management via the Juniper Apstra platform

3. **Explore a Module with `ansible-doc`**:
   the `ansible-doc` command also provides the ability to review documentation.
   ```bash
   ansible-doc juniper.apstra.apstra_facts
   ```
   Use this to inspect module documentation, usage examples, and available parameters.

### 🧪 Optional Exercise

Try looking at different collections from Galaxy and test one of its modules on a device in your lab environment. For example:

1. Search for "arista"
2. Review the install notes and documentation for "arista.avd"
3. Install arista.avd in your environment and review the documentation