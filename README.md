# pulsar-reverse-proxy-thing

A modified OpenVPN Road Warrior installer with built-in Pulsar Reverse Proxy functionality. This allows you to use a VPS and a Site hosted machine instead of an RDP connection.

**EDUCATIONAL PURPOSES ONLY!**

---

## What This Does

This script sets up an OpenVPN server on your VPS and automatically configures port forwarding for Pulsar. Instead of using RDP, your Pulsar C2 traffic will be routed through the VPN connection.

Originally based on: https://github.com/Nyr/openvpn-install
Modified by: AXL.Monero

THIS README WAS GENERATED USING AI SO TAKE THIS WITH A GRAIN OF SALT!!!

---

## Requirements

- A VPS running:
  - Ubuntu 22.04 or higher
  - Debian 11 or higher
  - CentOS/AlmaLinux/Rocky Linux 9 or higher
  - Fedora (latest)
- Root access (sudo)
- Basic understanding of VPN and networking

---

## Installation

1. Download the script to your VPS:
```bash
wget https://raw.githubusercontent.com/yourusername/pulsar-reverse-proxy-thing/main/openvpn-setup.sh
chmod +x openvpn-setup.sh
```

2. Run the script as root:
```bash
sudo bash openvpn-setup.sh
```

3. Follow the prompts:
   - Select your network configuration
   - Choose OpenVPN protocol (UDP recommended)
   - Select DNS server
   - Create first client
   - Choose whether to enable Pulsar Reverse Proxy

4. The script will install all dependencies automatically:
   - OpenVPN
   - OpenSSL
   - CA certificates
   - iptables or firewalld

---

## Usage

### First Time Setup

When you run the script for the first time, you will:
1. Configure OpenVPN server settings
2. Create your first VPN client
3. Optionally enable Pulsar Reverse Proxy
4. Get a .ovpn client configuration file

### Running the Script Again

After installation, running the script again gives you these options:

```
1) Add a new client
2) Revoke an existing client
3) Remove OpenVPN
4) Exit
5) Add/Update Pulsar Reverse Proxy
6) Remove Pulsar Reverse Proxy
7) UNINSTALL ALL! (Remove OpenVPN + Pulsar + Everything)
```

---

## Pulsar Configuration

### On Your VPS:
1. Run the script and choose option 5 to add Pulsar Reverse Proxy
2. Select your Pulsar port (default: 4782)
3. Select target VPN client IP (default: 10.8.0.2)

### On Your Client Machine:
1. Connect to the VPN using the .ovpn file
2. Start Pulsar and listen on the port you selected
3. In your Pulsar builder, use:
   - Connection Address: YOUR_VPS_IP:YOUR_SELECTED_PORT

### Client Configuration File Location:
- Your home directory like idk /home/user/
- If not found, check /root/
- Check the console to see where it saved it

---

## Known Limitations

**IMPORTANT:** The only feature that will NOT work with Pulsar through this setup is getting the client's actual IP address.

- The client IP will show as UNKNOWN or the VPS IP instead of the real client IP
- All other Pulsar features work normally
- This is a limitation of the VPN tunneling method

---

## Uninstalling

To completely remove everything this script installed:

1. Run the script again:
```bash
sudo bash openvpn-setup.sh
```

2. Select option 7: "UNINSTALL ALL!"
3. Type "YES" to confirm
4. Everything will be removed:
   - OpenVPN server
   - All client certificates
   - Pulsar reverse proxy rules
   - All iptables/firewalld rules
   - All systemd services

---

## Troubleshooting

### Check OpenVPN Service Status:
```bash
systemctl status openvpn-server@server
```

### Restart OpenVPN Service:
```bash
systemctl restart openvpn-server@server
```

### Check iptables Rules:
```bash
iptables -t nat -L -n -v
```

### View OpenVPN Logs:
```bash
journalctl -u openvpn-server@server -f
```

---

## Security Notes

- Keep your .ovpn client files secure
- Only distribute them to trusted devices
- Revoke client certificates if a device is compromised
- Change Pulsar ports regularly
- Use strong passwords for your VPS

---

## Disclaimer

This tool is provided for EDUCATIONAL PURPOSES ONLY. The author is not responsible for any misuse or damage caused by this script. Use at your own risk and ensure you comply with all applicable laws and regulations in your jurisdiction.

---

## License

Original OpenVPN installer: MIT License (Copyright 2013 Nyr)
Pulsar modifications: Educational use only

---

## Support

For issues with the original OpenVPN installer, visit: https://github.com/Nyr/openvpn-install

For issues with Pulsar integration, open an issue in this repository.
