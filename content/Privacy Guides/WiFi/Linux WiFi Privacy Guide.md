>[!warning] THIS GUIDE IS CURRENTLY UNDER CONSTRUCTION - CONTINUE AT YOUR OWN RISK


> [!info] Objective Prevent public WiFi routers from identifying your device via hostname, MAC address, and mDNS broadcasts.

## What Information Leaks to Routers

When connecting to public WiFi, routers can see:

- **Hostname** - Broadcast via DHCP (often contains personal info)
- **MAC Address** - Hardware identifier (can track across networks)
- **mDNS Broadcasts** - Machine name via Avahi/Zeroconf
- **IP Address** - Assigned by router (unavoidable)
- **Traffic Patterns** - Connection timing, packet sizes

---

## 1. Disable DHCP Hostname Broadcasting

### Problem

Your Linux machine broadcasts its hostname via DHCP when acquiring an IP lease. Default hostnames often reveal personal information (names, nicknames).

### Solution: NetworkManager (Most Modern Distros)

> [!warning] Per-Connection Configuration NetworkManager cannot disable hostname broadcasting globally - must configure per connection.

#### Disable for All Existing Connections

```bash
# Disable hostname broadcast for all connections
for conn in $(nmcli -t -f NAME connection show); do
    nmcli connection modify "$conn" ipv4.dhcp-send-hostname no
    nmcli connection modify "$conn" ipv6.dhcp-send-hostname no
done

# Reload NetworkManager
nmcli general reload
```

#### Disable for Specific WiFi Network

```bash
# List your connections
nmcli connection show

# Disable for specific connection (replace "WiFi-Name" with your SSID)
nmcli connection modify "WiFi-Name" ipv4.dhcp-send-hostname no
nmcli connection modify "WiFi-Name" ipv6.dhcp-send-hostname no

# Reconnect
nmcli connection down "WiFi-Name"
nmcli connection up "WiFi-Name"
```

#### Verify Settings

```bash
# Check if hostname sending is disabled
nmcli connection show "WiFi-Name" | grep dhcp-send-hostname
```

### Solution: dhclient (Legacy/Non-NetworkManager Systems)

Edit DHCP client configuration:

```bash
sudo nano /etc/dhcp/dhclient.conf
```

Comment out the hostname broadcast line:

```conf
# Find this line:
send host-name = gethostname();

# Change to:
#send host-name = gethostname();
```

---

## 2. Enable MAC Address Randomization

### Why This Matters

MAC addresses uniquely identify your hardware. Randomization prevents cross-network tracking and location profiling.

### Configuration

Create NetworkManager configuration file:

```bash
sudo nano /etc/NetworkManager/conf.d/00-macrandomize.conf
```

Add the following content:

```ini
[device]
# Randomize MAC during WiFi scanning
wifi.scan-rand-mac-address=yes

[connection]
# MAC randomization mode
wifi.cloned-mac-address=random
ethernet.cloned-mac-address=random
connection.stable-id=${CONNECTION}/${BOOT}
```

### MAC Randomization Modes

|Mode|Behavior|Use Case|
|---|---|---|
|`random`|New MAC every connection|Maximum privacy on public WiFi|
|`stable`|Same MAC per network|Captive portals, DHCP reservations|
|`permanent`|Use real MAC|Home network, MAC filtering|

> [!tip] Recommendation Use `random` for public WiFi, `stable` for networks with captive portals that remember your device.

### Apply Changes

```bash
# Restart NetworkManager
sudo systemctl restart NetworkManager

# Verify MAC randomization is working
ip link show wlan0
# Disconnect and reconnect - MAC should change with 'random' mode
```

---

## 3. Disable Avahi/mDNS Broadcasting

### Problem

Avahi broadcasts your machine name and services on each network via mDNS (Multicast DNS), even when no services are being published. This reveals your hostname to all devices on the network.

### Option A: Completely Disable Avahi (Recommended for Public WiFi)

```bash
# Stop the service
sudo systemctl stop avahi-daemon

# Prevent auto-start on boot
sudo systemctl disable avahi-daemon

# Mask to prevent activation via sockets
sudo systemctl mask avahi-daemon
sudo systemctl mask avahi-daemon.socket
```

### Option B: Configure Avahi to Not Publish (Keep mDNS Browsing)

If you need to discover other services on the network but don't want to broadcast your own:

```bash
sudo nano /etc/avahi/avahi-daemon.conf
```

Modify the `[publish]` section:

```ini
[publish]
# Do not publish workstation name
publish-workstation=no

# Disable OS/CPU info broadcast
publish-hinfo=no

# Disable IP address broadcast
publish-addresses=no

# Disable domain name broadcast
publish-domain=no

# Disable user service publishing
disable-user-service-publishing=yes

# Browse-only mode (safest option)
disable-publishing=yes
```

Restart Avahi:

```bash
sudo systemctl restart avahi-daemon
```

---

## 4. Change Hostname to Generic Value

Set a non-identifying hostname:

```bash
# Change to something generic
sudo hostnamectl set-hostname generic-laptop

# Or use a more creative generic name
sudo hostnamectl set-hostname laptop-ubuntu

# Verify change
hostnamectl
```

> [!note] Hostname Persistence This change persists across reboots. Change it back when needed for your home network.

---

## 5. Verification & Testing

### Check MAC Address Randomization

```bash
# View current MAC address
ip link show wlan0

# Look for "link/ether" - should change after disconnect/reconnect with 'random' mode
```

### Monitor DHCP Requests

```bash
# Install tcpdump if not present
sudo apt install tcpdump  # Debian/Ubuntu
sudo pacman -S tcpdump    # Arch

# Monitor DHCP traffic (requires root)
sudo tcpdump -vvvn port bootps

# Look for "Hostname Option 12" - should be absent or generic
```

### Check mDNS Broadcasts

```bash
# Install avahi-utils if needed
sudo apt install avahi-utils  # Debian/Ubuntu
sudo pacman -S avahi          # Arch

# Browse for mDNS services
avahi-browse -a

# Your device should NOT appear if Avahi is disabled/configured correctly
```

### What the Router Sees After Configuration

✅ **Hidden:**

- Random MAC address (changes per connection or network)
- Generic hostname (if sent at all)
- No mDNS/Avahi broadcasts

❌ **Still Visible:**

- IP address (assigned by router - unavoidable)
- Traffic patterns (timing, packet sizes, protocols)
- Connection duration

---

## Complete Configuration Summary

### File: `/etc/NetworkManager/conf.d/00-macrandomize.conf`

```ini
[device]
wifi.scan-rand-mac-address=yes

[connection]
wifi.cloned-mac-address=random
ethernet.cloned-mac-address=random
connection.stable-id=${CONNECTION}/${BOOT}
```

### Per-Connection Settings

```bash
# For each public WiFi network
nmcli connection modify "CoffeeShop-WiFi" ipv4.dhcp-send-hostname no
nmcli connection modify "CoffeeShop-WiFi" ipv6.dhcp-send-hostname no
```

### Avahi Configuration

```bash
# Complete disable (simplest)
sudo systemctl mask avahi-daemon avahi-daemon.socket
```

### Hostname

```bash
sudo hostnamectl set-hostname generic-device
```

---

## Quick Setup Script

```bash
#!/bin/bash
# Linux WiFi Privacy Setup

echo "Setting up WiFi privacy configuration..."

# 1. Create MAC randomization config
echo "Creating MAC randomization config..."
sudo tee /etc/NetworkManager/conf.d/00-macrandomize.conf > /dev/null <<EOF
[device]
wifi.scan-rand-mac-address=yes

[connection]
wifi.cloned-mac-address=random
ethernet.cloned-mac-address=random
connection.stable-id=\${CONNECTION}/\${BOOT}
EOF

# 2. Disable hostname broadcast for all connections
echo "Disabling DHCP hostname broadcast..."
for conn in $(nmcli -t -f NAME connection show); do
    nmcli connection modify "$conn" ipv4.dhcp-send-hostname no 2>/dev/null
    nmcli connection modify "$conn" ipv6.dhcp-send-hostname no 2>/dev/null
done

# 3. Disable Avahi
echo "Disabling Avahi mDNS..."
sudo systemctl stop avahi-daemon 2>/dev/null
sudo systemctl disable avahi-daemon 2>/dev/null
sudo systemctl mask avahi-daemon 2>/dev/null
sudo systemctl mask avahi-daemon.socket 2>/dev/null

# 4. Set generic hostname
echo "Setting generic hostname..."
sudo hostnamectl set-hostname generic-laptop

# 5. Restart NetworkManager
echo "Restarting NetworkManager..."
sudo systemctl restart NetworkManager

echo "Privacy configuration complete!"
echo "Reconnect to WiFi for changes to take effect."
```

Save as `wifi-privacy-setup.sh`, make executable:

```bash
chmod +x wifi-privacy-setup.sh
sudo ./wifi-privacy-setup.sh
```

---

## Additional Privacy Layers

### VPN (Recommended)

While VPN doesn't hide device identity from the router, it:

- ✅ Encrypts all traffic content
- ✅ Hides browsing activity from coffee shop
- ✅ Prevents ISP/router owner from monitoring sites visited
- ❌ Router still sees: MAC, connection existence, VPN server IP

### Firewall Rules (Block mDNS at Network Level)

```bash
# UFW (Ubuntu/Debian)
sudo ufw deny out 5353/udp comment "block mDNS"

# FirewallD (Fedora/RHEL)
sudo firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 0 -p udp --dport=5353 -j DROP
sudo firewall-cmd --permanent --direct --add-rule ipv6 filter OUTPUT 0 -p udp --dport=5353 -j DROP
sudo firewall-cmd --reload
```

---

## Limitations & Considerations

> [!warning] Known Limitations
> 
> - **Behavioral Analysis**: Traffic patterns can still correlate devices across networks
> - **Corporate WiFi**: MAC filtering may reject randomized addresses
> - **Captive Portals**: May require `stable` MAC mode instead of `random`
> - **Network Services**: Some services (printer discovery, file sharing) may not work with Avahi disabled

### When NOT to Use These Settings

- Home network with static DHCP assignments
- Corporate network with MAC whitelisting
- IoT device discovery requirements
- Network printing setup needed

---

## Troubleshooting

### MAC Randomization Not Working

```bash
# Check NetworkManager version (requires 1.4.0+)
nmcli --version

# Check if backend is iwd (doesn't support MAC randomization via NM)
nmcli device status

# If using iwd backend, configure in /etc/iwd/main.conf instead
sudo nano /etc/iwd/main.conf
```

Add to iwd configuration:

```ini
[General]
AddressRandomization=once
AddressRandomizationRange=nic
```

### Connection Issues with Random MAC

Some networks may have issues. Switch to `stable` mode:

```bash
# Edit config file
sudo nano /etc/NetworkManager/conf.d/00-macrandomize.conf

# Change 'random' to 'stable'
wifi.cloned-mac-address=stable
```

### Avahi Auto-Restarts

If Avahi keeps restarting despite being disabled:

```bash
# Some services (like CUPS) can trigger Avahi via D-Bus
# Mask both the service and socket
sudo systemctl mask avahi-daemon.service
sudo systemctl mask avahi-daemon.socket

# Verify it's masked
systemctl status avahi-daemon
```

---

## References & Sources

- NetworkManager Documentation: [nm-settings(5)](https://networkmanager.dev/docs/api/latest/nm-settings.html)
- Arch Linux Wiki: [NetworkManager](https://wiki.archlinux.org/title/NetworkManager)
- Fedora Magazine: [MAC Address Randomization](https://fedoramagazine.org/randomize-mac-address-nm/)
- Avahi Documentation: [Avahi.org](http://avahi.org/)

---

## Tags

#linux #privacy #networking #wifi #security #anonymity #dhcp #mac-address #mdns

---

**Last Updated:** 2026-01-04  
**Confidence Level:** High  
**Tested On:** Debian 12, Ubuntu 22.04/24.04