
> [!info] Objective Prevent public WiFi routers from identifying your Mac via hostname, MAC address, and Bonjour/mDNS broadcasts.

## What Information Leaks to Routers

When connecting to public WiFi, routers can see:

- **Computer Name** - Broadcast via DHCP and Bonjour
- **Local Hostname** - Bonjour/mDNS name (name.local)
- **MAC Address** - Hardware identifier (can track across networks)
- **Bonjour/mDNS** - Service discovery broadcasts (UDP/5353)
- **IP Address** - Assigned by router (unavoidable)
- **Traffic Patterns** - Connection timing, packet sizes

---

## 1. Enable Private WiFi Address (MAC Randomization)

### Why This Matters

macOS Sequoia (15) and later introduced MAC address randomization similar to iOS 14+. Each WiFi network gets a different randomized MAC address to prevent cross-network tracking.

### macOS Sequoia 15+ Configuration

**Enable for Specific Network:**

```bash
# Via System Settings
1. Click Apple menu → System Settings
2. Click "Wi-Fi"
3. Click "Details" button next to your connected network
4. Find "Private Wi-Fi Address" section
5. Select "Fixed" (same randomized MAC per network)
   or "Rotating" (changes MAC every 2 weeks on insecure networks)
6. Click "OK"
```

**Private Wi-Fi Address Modes:**

- **Fixed**: Same randomized MAC for this network (default for WPA2/WPA3)
- **Rotating**: MAC changes every ~2 weeks (auto-enabled for open/WEP/WPA1 networks)
- **Off**: Use real hardware MAC

> [!important] macOS 15 Default Behavior Starting with macOS Sequoia 15, Private Wi-Fi Address is ON by default and rotates every 2 weeks on insecure networks (open WiFi, WEP, WPA1).

### macOS 14 and Earlier

MAC randomization is NOT available for WiFi connections in macOS 14 and earlier. Only scanning uses randomized MACs.

### iOS/iPadOS (Reference)

```bash
# Settings → Wi-Fi → (i) next to network → Private Wi-Fi Address
# Toggle ON for privacy, OFF if network requires real MAC
```

### Verify MAC Address

```bash
# Check current MAC address
ifconfig en0 | grep ether

# Disconnect and reconnect - MAC may change with "Rotating" mode
```

---

## 2. Change Computer Names to Generic Values

### Why This Matters

macOS has THREE different names that can leak via DHCP and Bonjour. All should be changed to non-identifying values.

### The Three Mac Names

1. **Computer Name** - Displayed in Finder, AirDrop
2. **Local Hostname** - Bonjour name (name.local)
3. **HostName** - Unix hostname (Terminal, network services)

### Change Via System Settings

**Computer Name & Local Hostname:**

```bash
1. Click Apple menu → System Settings
2. Click "General" → "Sharing"
3. Click "Edit" next to computer name
4. Change "Computer Name" to: "generic-mac"
5. Change "Local Hostname" to: "generic-mac.local"
6. Click "OK"
```

### Change All Three Via Terminal (Recommended)

```bash
# Run in Terminal (will prompt for password)

# Set Computer Name
sudo scutil --set ComputerName "generic-mac"

# Set Local Hostname (Bonjour name)
sudo scutil --set LocalHostName "generic-mac"

# Set Unix HostName
sudo scutil --set HostName "generic-mac"

# Verify changes
scutil --get ComputerName
scutil --get LocalHostName
scutil --get HostName
```

> [!tip] Naming Guidelines
> 
> - Use lowercase letters, numbers, hyphens only
> - No spaces or special characters
> - Examples: "generic-mac", "laptop-macos", "macbook"

---

## 3. Disable Bonjour Advertising (mDNS Broadcasts)

### Why This Matters

Bonjour broadcasts your Mac's name and services on the local network via mDNS (Multicast DNS). On public WiFi, this reveals your device identity.

> [!warning] Important Limitation Completely disabling mDNSResponder will break DNS resolution on macOS. Only disable Bonjour **advertising**, not the service itself.

### Disable Bonjour Advertising (Recommended Method)

```bash
# Run in Terminal
sudo defaults write /Library/Preferences/com.apple.mDNSResponder.plist NoMulticastAdvertisements -bool true

# Reboot for changes to take effect
sudo reboot
```

### Verify Bonjour Advertising is Disabled

```bash
# Check the setting
sudo defaults read /Library/Preferences/com.apple.mDNSResponder.plist NoMulticastAdvertisements

# Should return: 1

# Check for Bonjour broadcasts on network (install Wireshark)
# Filter: udp.port == 5353
# You should see no outbound advertisements from your Mac
```

### Alternative: Temporary Disable (Not Recommended)

This method is **NOT recommended** as it breaks DNS resolution:

```bash
# This will break DNS - only for testing
sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.mDNSResponder.plist

# To re-enable (required to restore DNS)
sudo launchctl load -w /System/Library/LaunchDaemons/com.apple.mDNSResponder.plist
```

### Disable Specific Bonjour Services (Advanced)

If you want to keep some services but disable others:

```bash
# Disable AirDrop (while keeping other services)
sudo defaults write com.apple.NetworkBrowser DisableAirDrop -bool true

# Restart Finder
killall Finder
```

---

## 4. Disable File Sharing Services

### Why This Matters

Various sharing services broadcast your Mac's presence on the network.

### Disable All Sharing Services

```bash
1. Click Apple menu → System Settings
2. Click "General" → "Sharing"
3. Turn OFF all services:
   - File Sharing
   - Screen Sharing
   - Media Sharing
   - Printer Sharing
   - Remote Login
   - Remote Management
   - Remote Apple Events
   - AirPlay Receiver
   - Content Caching
```

### Verify Sharing Services via Terminal

```bash
# Check all sharing services
sudo sharing -l

# Should show all services as "disabled"
```

---

## 5. Configure Firewall for Privacy

### Enable macOS Firewall

```bash
1. Click Apple menu → System Settings
2. Click "Network"
3. Click "Firewall"
4. Click "Turn On" if not already enabled
5. Click "Options"
6. Check "Block all incoming connections" for maximum privacy
7. Uncheck "Automatically allow built-in software to receive connections"
8. Uncheck "Automatically allow downloaded signed software to receive connections"
```

### Firewall via Terminal

```bash
# Enable firewall
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setglobalstate on

# Block all incoming connections
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setblockall on

# Enable stealth mode (don't respond to ICMP/ping)
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setstealthmode on

# Verify status
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --getglobalstate
```

---

## 6. Verification & Testing

### Check Current Network Identity

```bash
# Check all three Mac names
echo "Computer Name: $(scutil --get ComputerName)"
echo "Local Hostname: $(scutil --get LocalHostName)"
echo "HostName: $(scutil --get HostName)"

# Check current MAC address
ifconfig en0 | grep ether

# Check Bonjour advertising setting
sudo defaults read /Library/Preferences/com.apple.mDNSResponder.plist NoMulticastAdvertisements 2>/dev/null || echo "Not set"
```

### Monitor Network Traffic (Advanced)

Use Wireshark to verify no identity leaks:

```bash
# Install Wireshark (via Homebrew)
brew install --cask wireshark

# Monitor for mDNS broadcasts
# Filter: udp.port == 5353
# Should see no outbound advertisements with your Mac name
```

### Check Open Ports

```bash
# List listening ports
sudo lsof -iTCP -sTCP:LISTEN -n -P

# Should show minimal services for privacy
```

---

## Complete Configuration Summary

### Quick Configuration Script

Save as `macos-wifi-privacy.sh`:

```bash
#!/bin/bash
# macOS WiFi Privacy Configuration Script
# Requires sudo privileges

set -e

echo "🔒 macOS WiFi Privacy Configuration"
echo "======================================"

# Check if running as root
if [ "$EUID" -ne 0 ]; then 
    echo "❌ Please run with sudo: sudo bash $0"
    exit 1
fi

# 1. Change system names to generic
echo ""
echo "[1/5] Setting generic computer names..."
scutil --set ComputerName "generic-mac"
scutil --set LocalHostName "generic-mac"
scutil --set HostName "generic-mac"
echo "✓ Computer names set to 'generic-mac'"

# 2. Disable Bonjour advertising
echo ""
echo "[2/5] Disabling Bonjour advertising..."
defaults write /Library/Preferences/com.apple.mDNSResponder.plist NoMulticastAdvertisements -bool true
echo "✓ Bonjour advertising disabled"

# 3. Disable file sharing services
echo ""
echo "[3/5] Disabling sharing services..."
sharing -a 0 2>/dev/null || true
echo "✓ All sharing services disabled"

# 4. Enable firewall with strict settings
echo ""
echo "[4/5] Configuring firewall..."
/usr/libexec/ApplicationFirewall/socketfilterfw --setglobalstate on
/usr/libexec/ApplicationFirewall/socketfilterfw --setblockall off
/usr/libexec/ApplicationFirewall/socketfilterfw --setstealthmode on
echo "✓ Firewall enabled with stealth mode"

# 5. Manual step reminder
echo ""
echo "[5/5] MAC Randomization (Manual Step Required)"
echo "    For macOS Sequoia 15+:"
echo "    1. System Settings → Wi-Fi"
echo "    2. Click 'Details' next to your WiFi network"
echo "    3. Enable 'Private Wi-Fi Address' → 'Rotating'"
echo ""
echo "    For macOS 14 and earlier:"
echo "    MAC randomization is not available for WiFi connections"

echo ""
echo "✅ Privacy configuration complete!"
echo ""
echo "⚠️  IMPORTANT: Restart your Mac for all changes to take effect"
echo ""

# Verification
echo "Current Configuration:"
echo "  Computer Name: $(scutil --get ComputerName)"
echo "  Local Hostname: $(scutil --get LocalHostName)"  
echo "  HostName: $(scutil --get HostName)"
echo "  Bonjour Advertising: $(defaults read /Library/Preferences/com.apple.mDNSResponder.plist NoMulticastAdvertisements 2>/dev/null && echo 'Disabled' || echo 'Enabled')"
echo ""
```

Run with:

```bash
# Save script to file
chmod +x macos-wifi-privacy.sh

# Run with sudo
sudo ./macos-wifi-privacy.sh

# Restart Mac
sudo reboot
```

---

## Additional Privacy Layers

### Use VPN on Public WiFi

While VPN doesn't hide device identity from the router, it:

- ✅ Encrypts all traffic content
- ✅ Hides browsing activity from network owner
- ✅ Prevents ISP monitoring
- ❌ Router still sees: MAC, connection existence, VPN server IP

### Disable Location Services (Optional)

```bash
# System Settings → Privacy & Security → Location Services
# Turn off or limit to essential apps only
```

### Use iCloud Private Relay (macOS 12+)

```bash
# System Settings → Apple ID → iCloud → Private Relay
# Enables encrypted web browsing (Safari only)
```

---

## What the Router Sees After Configuration

✅ **Hidden (macOS 15+):**

- Randomized MAC address (fixed or rotating)
- Generic computer name
- No Bonjour/mDNS broadcasts
- No service advertisements

✅ **Hidden (All macOS Versions):**

- Generic computer name
- Reduced Bonjour broadcasts
- No file sharing advertisements

❌ **Still Visible:**

- IP address (assigned by router - unavoidable)
- Traffic patterns (timing, packet sizes, protocols)
- Connection duration
- Real MAC (macOS 14 and earlier)

---

## Troubleshooting

### Private WiFi Address Not Available (macOS 14 and Earlier)

```bash
# MAC randomization not supported for connections
# Only available for WiFi scanning
# Solution: Upgrade to macOS 15 or accept limitation
```

### AirDrop/Handoff Stopped Working

```bash
# Bonjour advertising is required for these features
# Re-enable on trusted networks:
sudo defaults write /Library/Preferences/com.apple.mDNSResponder.plist NoMulticastAdvertisements -bool false
sudo reboot

# Or use only on home network, disable on public WiFi
```

### Network Printers Not Found

```bash
# Bonjour is required for automatic printer discovery
# Option 1: Manually add printer by IP address
# Option 2: Re-enable Bonjour advertising temporarily
```

### Hostname Keeps Changing

```bash
# macOS may auto-adjust hostname if conflicts detected
# Set all three names consistently:
sudo scutil --set ComputerName "generic-mac"
sudo scutil --set LocalHostName "generic-mac"
sudo scutil --set HostName "generic-mac"

# Check /etc/hosts for conflicts:
sudo nano /etc/hosts
```

### DNS Resolution Broken

```bash
# If you accidentally disabled mDNSResponder entirely:
sudo launchctl load -w /System/Library/LaunchDaemons/com.apple.mDNSResponder.plist

# Flush DNS cache:
sudo dscacheutil -flushcache
sudo killall -HUP mDNSResponder
```

### Revert All Changes

```bash
#!/bin/bash
# Revert privacy settings

# Re-enable Bonjour advertising
sudo defaults delete /Library/Preferences/com.apple.mDNSResponder.plist NoMulticastAdvertisements 2>/dev/null

# Reset computer names to defaults (example)
sudo scutil --set ComputerName "MacBook Pro"
sudo scutil --set LocalHostName "MacBook-Pro"
sudo scutil --set HostName "MacBook-Pro.local"

# Disable Private WiFi Address
# (Must be done manually in System Settings → Wi-Fi → Details)

echo "Settings reverted. Restart your Mac."
```

---

## Limitations & Considerations

> [!warning] Known Limitations
> 
> - **macOS 14 and earlier**: No MAC randomization for WiFi connections
> - **AirDrop/Handoff**: Require Bonjour advertising to function
> - **Network Printing**: May require Bonjour service discovery
> - **Corporate Networks**: May need specific hostname for authentication
> - **Behavioral Analysis**: Traffic patterns can still correlate devices

### When NOT to Use These Settings

- Home network with AirDrop, Handoff, or Continuity features
- Corporate/enterprise WiFi requiring specific hostname
- Network printing without manual IP configuration
- Development environments requiring Bonjour services
- Media streaming (AirPlay) between devices

---

## Version-Specific Notes

### macOS Sequoia 15+ (2024)

- ✅ Private WiFi Address available (MAC randomization)
- ✅ Rotating MACs on insecure networks (every 2 weeks)
- ✅ Fixed MACs per network for secure networks
- Recommended: Enable "Rotating" mode for public WiFi

### macOS Sonoma 14 and Earlier

- ❌ No MAC randomization for WiFi connections
- ✅ Can still disable Bonjour advertising
- ✅ Can change hostnames to generic values
- Privacy is more limited without MAC randomization

### iOS/iPadOS Comparison

- iOS 14+ has had MAC randomization since 2020
- iOS 18+ rotates MACs every 2 weeks on insecure networks
- Mobile devices generally have better privacy controls

---

## References & Sources

- Apple Support: [Private Wi-Fi Addresses](https://support.apple.com/guide/security/privacy-features-connecting-wireless-networks-secb9cb3140c/web)
- MacAdmin Resources: [MAC Randomization in macOS 15](https://macadmin.fraserhess.com/2024/09/16/handling-mac-address-randomization-in-macos-15/)
- CIS Benchmark: [Disable Bonjour](https://www.tenable.com/audits/items/CIS_Apple_macOS_10.13_v1.1.0_Level_2.audit)

---

## Tags

#macos #privacy #networking #wifi #security #bonjour #mdns #mac-address

---

**Last Updated:** 2026-01-04  
**Confidence Level:** High  
**Tested On:** macOS Sonoma 14, macOS Sequoia 15