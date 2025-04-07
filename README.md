# Bluetooth Auto-Connect Service

Automatically connects to a specified Bluetooth device on system startup using `bluetoothctl`.

## 📦 Prerequisites
- Linux with `bluetoothctl` (typically included in `bluez` package)
- Active Bluetooth adapter
- Pre-paired target device

## 🛠 Installation
```bash
git clone https://github.com/yourusername/bluetooth-auto-connect.git
cd bluetooth-auto-connect
chmod +x scripts/bt_connect.sh
sudo cp scripts/bt_connect.sh /usr/local/bin/
sudo cp systemd/bluetooth-auto-connect.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable --now bluetooth-auto-connect.service

⚙ Configuration
Edit MAC address in:
nano scripts/bt_connect.sh  # Change DEVICE_MAC value

📚 Documentation
Troubleshooting Guide

Systemd Service Reference

🤝 Contributing
Fork the repository

Create your feature branch

Commit changes

Push to the branch

Open a pull request

📜 License
MIT License


---

**Файл 2: TROUBLESHOOTING.md**
```markdown
# Troubleshooting Guide

## 🔍 Basic Checks
1. Verify Bluetooth is active:
   ```bash
   rfkill list bluetooth
   sudo rfkill unblock bluetooth

Check device pairing status:

bluetoothctl
paired-devices

🚨 Common Issues
1. Connection Timeout
Symptoms: Service starts but device doesn't connect
Solutions:

Increase wait time in service file:
ExecStartPre=/bin/sleep 30  # Increased from 20
Manual connection test:
echo -e "connect XX:XX:XX:XX:XX:XX\nquit" | bluetoothctl

2. Service Failure
Symptoms: Service crashes repeatedly
Debug steps:

journalctl -u bluetooth-auto-connect.service -f -n 50
sudo systemctl reset-failed bluetooth-auto-connect.service

3. Permission Issues
Fix:

sudo chown root:root /usr/local/bin/bt_connect.sh
sudo chmod 755 /usr/local/bin/bt_connect.sh

📋 Advanced Diagnostics
Monitor Bluetooth logs:

sudo btmon

Test without service:

/usr/local/bin/bt_connect.sh

Re-pair device:

bluetoothctl
remove XX:XX:XX:XX:XX:XX
pair XX:XX:XX:XX:XX:XX
trust XX:XX:XX:XX:XX:XX


