## 📦 Prerequisites

Install required packages
```bash
sudo dnf install -y python3-pip sshpass python3 git
```

Install required collections:
```bash
ansible-galaxy collection install -r requirements.yml
```

Install Python dependencies:
```bash
pip install -r requirements.txt
```

### Instructions for getting the class code to the local machine:
1. Open Visual Studio Code
2. View -> Command Palette
3. Remote-SSH: Connect to Host:
4. Select: 10.8.4.10
5. Enter password from "credentials" document
6. if prompted, enter "linux" as the remote operating system
7. Left hand side bar: "Open folder"
8. `/home/labuser/` should be selected
9. Click "OK"
10. Enter password again
11. In the resulting "welcome" page, select "clone git repository"
12. Select "Clone from GitHub"
13. Enter: https://github.com/myriad-360/Ansible-Delivery-Training-Student.git
14. Select the current directory as the destination directory to clone the repository to
15. Would you like to open the cloned repository, or add it to the current workspacee
16. Add the cloned repository to the workspace
