# Ansible Network Automation Training – Student Index

Welcome! Below are the step-by-step exercises for learning Ansible in a network automation context. Click each step to access the task, code, and explanation.

---

## 📘 Training Modules

0. [M0 – Prerequisites](./M0-Prerequisites)  
   *Prepare your environment and ensure all dependencies are installed before beginning the training.*  
   This module includes `requirements.txt` and `requirements.yml` to install Python and Ansible collections.

1. [M1 – Install Ansible and Understand Inventory](./M1-Install_Ansible_and_Understand_Inventory)  
   *Install Ansible and learn how to use inventory files to define managed devices.*  
   Explore multiple inventory formats (`.ini`, `.yml`) to define hosts and groups.

2. [M2 – Run Ad Hoc Commands](./M2-Run_Ad_Hoc_Commands)  
   *Use Ansible ad hoc commands for quick, one-time tasks without writing a playbook.*  

3. [M3 – Write and Execute Playbooks](./M3-Write_And_Execute_Playbooks)  
   *Create and run your first Ansible playbooks to automate network tasks.*  
   Discusses sample playbooks like `show_config.yml`, `show_facts.yml`, and `show_ntp.yml`.

4. [M4 – Variables and Templates](./M4-Variables_and_Templates)  
   *Parameterize configurations using variables and Jinja2 templates.*  
   You'll use `group_vars/` and `templates/` directories to generate NTP configurations dynamically.

5. [M5 – Loops and Conditionals](./M5-Loops_and_Conditionals)  
   *Add logic and iteration to your playbooks for more dynamic automation.*  
   Covers use of `loop` and `when` directives in tasks like creating loopback interfaces and address objects.

6. [M6 – Secrets with Ansible Vault](./M6-Secrets_with_Ansible_Vault)  
   *Protect sensitive data like passwords using Ansible Vault.*  
   Demonstrates how to encrypt secrets and use them in playbooks (`gather_device_facts_with_vault.yml`).

7. [M7 – Complex Workflows](./M7-Complex_Workflows)  
   *Chain multiple tasks and roles together to create advanced automation flows.*  
   Discusses `complex_workflow.yml` and a `tasks/` directory to structure logic and execution.

8. [M8 – Reusable Frameworks](./M8-Reusable_Frameworks)  
   *Structure your automation using roles and collections for reuse and scale.*  
   Explore the use of roles with `roles/` directory and `run_roles.yml` playbook.

9. [M9 – Explore Extra Tooling](./M9-Explore_Extra_Tooling)  
   *Discover tools like `ansible-doc`, Galaxy, and explore third-party modules.*  
   Provides guidance for exploring new modules and collections not included by default.

---

📦 These files are meant to be used as standalone references or as part of a guided training session.  
💡 Make sure you have Ansible installed and configured before starting.
