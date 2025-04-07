1. README.md

markdown

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
```
⚙ Configuration
Edit MAC address in:

```bash
nano scripts/bt_connect.sh  # Change DEVICE_MAC value
```

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

**2. TROUBLESHOOTING.md**
```markdown
# Troubleshooting Guide
```
## 🔍 Basic Checks
1. Verify Bluetooth is active:
   ```bash
   rfkill list bluetooth
   sudo rfkill unblock bluetooth
   ```
Check device pairing status:

```bash
bluetoothctl
paired-devices
```
🚨 Common Issues
1. Connection Timeout
Symptoms: Service starts but device doesn't connect
Solutions:

Increase wait time in service file:

```ini
ExecStartPre=/bin/sleep 30  # Increased from 20
```
Manual connection test:

```bash
echo -e "connect XX:XX:XX:XX:XX:XX\nquit" | bluetoothctl
```

2. Service Failure
Symptoms: Service crashes repeatedly
Debug steps:

```bash
journalctl -u bluetooth-auto-connect.service -f -n 50
sudo systemctl reset-failed bluetooth-auto-connect.service
```

3. Permission Issues
Fix:

```bash
sudo chown root:root /usr/local/bin/bt_connect.sh
sudo chmod 755 /usr/local/bin/bt_connect.sh
```

**3. scripts/bt_connect.sh**
```bash
#!/bin/bash
# Bluetooth auto-connect script
DEVICE_MAC="XX:XX:XX:XX:XX:XX"  # Replace with your device's MAC address

echo -e "connect $DEVICE_MAC\nquit" | bluetoothctl
```

4. systemd/bluetooth-auto-connect.service

```ini
[Unit]
Description=Bluetooth Auto-Connect Service
After=bluetooth.target network.target
Requires=bluetooth.service

[Service]
Type=simple
ExecStartPre=/bin/sleep 20
ExecStart=/usr/local/bin/bt_connect.sh
Restart=on-failure
RestartSec=5s

[Install]
WantedBy=multi-user.target
```
