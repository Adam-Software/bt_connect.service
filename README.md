# Bluetooth Auto-Connect Service

This service automatically connects to a specified Bluetooth device on system startup.

## Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/bluetooth-auto-connect.git
Make the script executable:

chmod +x scripts/bt_connect.sh
Copy the script to /usr/local/bin:

sudo cp scripts/bt_connect.sh /usr/local/bin/
Copy the service file to systemd:

sudo cp systemd/bluetooth-auto-connect.service /etc/systemd/system/
Enable and start the service:

sudo systemctl daemon-reload
sudo systemctl enable bluetooth-auto-connect.service
sudo systemctl start bluetooth-auto-connect.service
Configuration
Edit bt_connect.sh to change the Bluetooth device MAC address (replace 67:70:08:66:36:77 with your device's address).

Troubleshooting
Check service status:

systemctl status bluetooth-auto-connect.service
View logs:

journalctl -u bluetooth-auto-connect.service -b
Copy

2. **scripts/bt_connect.sh** (your existing script, slightly improved):
```bash
#!/bin/bash
# Bluetooth auto-connect script
DEVICE_MAC="67:70:08:66:36:77"  # Replace with your device's MAC address

echo -e "connect $DEVICE_MAC\nquit" | bluetoothctl
systemd/bluetooth-auto-connect.service (your service file, slightly improved):

Description=Bluetooth Auto-Connect Service
After=bluetooth.target network.target
Requires=bluetooth.service

Type=simple
ExecStartPre=/bin/sleep 20  # Wait for Bluetooth to initialize
ExecStart=/usr/local/bin/bt_connect.sh
Restart=on-failure
RestartSec=5s

WantedBy=multi-user.target
