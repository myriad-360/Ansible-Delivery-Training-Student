# Step 2: Run an Ad Hoc Command to Show Version

## 🧪 Objective
Check a device's operating system version without a playbook.

## 📜 Code to Execute

```bash
ansible r1 -m ios_command -a "commands=['show version']" -c network_cli
```

## 🧠 What This Does
- Uses the `ios_command` module to run a CLI command
- `-c network_cli` specifies connection type for network devices

## 🧩 Why This Matters
Ad hoc commands are fast and flexible — great for testing, verification, or simple changes.