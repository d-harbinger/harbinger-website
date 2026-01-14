>[!warning] THIS GUIDE IS CURRENTLY UNDER CONSTRUCTION - CONTINUE AT YOUR OWN RISK


> [!info] Objective Prevent public WiFi routers from identifying your Windows device via hostname, MAC address, NetBIOS, LLMNR, and mDNS broadcasts.

## What Information Leaks to Routers

When connecting to public WiFi, routers can see:

- **Computer Name** - Broadcast via DHCP and NetBIOS
- **MAC Address** - Hardware identifier (can track across networks)
- **NetBIOS Broadcasts** - Legacy name resolution (UDP/137, 138, 139)
- **LLMNR** - Link-Local Multicast Name Resolution (UDP/5355)
- **mDNS** - Multicast DNS (UDP/5353) - Windows 10/11
- **IP Address** - Assigned by router (unavoidable)
- **Traffic Patterns** - Connection timing, packet sizes

---

## 1. Enable MAC Address Randomization

### Why This Matters

Windows 11 can randomize the MAC address to improve privacy and security of wireless connections, preventing tracking across public networks.

### Windows 11 Configuration

#### Enable for Specific WiFi Network (Recommended)

```powershell
# Via Settings GUI
1. Press Windows + I to open Settings
2. Click "Network & internet"
3. Click "Wi-Fi"
4. Click "Properties" for your active connection
5. Scroll to "Random hardware addresses"
6. Choose:
   - "On" - Keeps same random MAC for this network
   - "Change daily" - New MAC address every 24 hours
   - "Off" - Use real MAC address
```

**Recommendation:**

- Use "Change daily" for maximum privacy on public WiFi
- Use "On" for networks with captive portals
- Use "Off" only for home networks requiring MAC whitelisting

#### Enable for All WiFi Networks

```powershell
# Via Settings GUI
1. Press Windows + I to open Settings
2. Click "Network & internet"
3. Click "Wi-Fi"
4. Turn ON "Random hardware addresses" toggle
```

> [!tip] MAC Randomization Modes
> 
> - **On**: Same randomized MAC per network (persistent)
> - **Change Daily**: New MAC every 24 hours (maximum privacy)
> - **Off**: Use permanent hardware MAC

### Windows 10 Configuration

Same steps as Windows 11. MAC randomization is disabled by default in Windows, so only enable it if desired for privacy.

### Verify MAC Randomization

```powershell
# View current MAC address
ipconfig /all

# Look for "Physical Address" under your WiFi adapter
# It should change after reconnecting with "Change daily" mode
```

---

## 2. Change Computer Name to Generic Value

### Why This Matters

Your computer name is broadcast via DHCP, NetBIOS, and other protocols. Default names often contain personal information.

### Change Computer Name

**Method 1: Settings (Recommended)**

```powershell
1. Press Windows + I to open Settings
2. Click "System"
3. Click "About"
4. Click "Rename this PC"
5. Enter a generic name: "laptop-windows" or "generic-pc"
6. Restart when prompted
```

**Method 2: System Properties**

```powershell
1. Press Windows + R
2. Type: sysdm.cpl
3. Click "Change" under Computer Name tab
4. Enter new name
5. Click OK and restart
```

**Method 3: PowerShell (Advanced)**

```powershell
# Run as Administrator
Rename-Computer -NewName "generic-laptop" -Restart
```

---

## 3. Disable NetBIOS over TCP/IP

### Why This Matters

NetBIOS over TCP/IP broadcasts your computer name on the local network and can be exploited for tracking and attacks.

### Disable NetBIOS Per Network Adapter

**Via GUI:**

```powershell
1. Press Windows + R, type: ncpa.cpl
2. Right-click your WiFi adapter → Properties
3. Select "Internet Protocol Version 4 (TCP/IPv4)"
4. Click "Properties"
5. Click "Advanced"
6. Go to "WINS" tab
7. Select "Disable NetBIOS over TCP/IP"
8. Click OK
```

### Disable NetBIOS via PowerShell (All Adapters)

```powershell
# Run as Administrator
$regkey = "HKLM:SYSTEM\CurrentControlSet\services\NetBT\Parameters\Interfaces"
Get-ChildItem $regkey | ForEach-Object { 
    Set-ItemProperty -Path "$regkey\$($_.PSChildName)" -Name NetbiosOptions -Value 2 -Verbose
}
```

**NetbiosOptions Values:**

- `0` = Default (use DHCP setting)
- `1` = Enable NetBIOS
- `2` = Disable NetBIOS

### Verify NetBIOS is Disabled

```cmd
netstat -ano | find ":137"
netstat -ano | find ":138"
netstat -ano | find ":139"
```

If no output, NetBIOS is successfully disabled.

---

## 4. Disable LLMNR (Link-Local Multicast Name Resolution)

### Why This Matters

LLMNR allows hostname resolution on local networks without DNS, broadcasting your device name via UDP/5355.

### Disable via Registry

```powershell
# Run as Administrator
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\DNSClient" /v EnableMulticast /t REG_DWORD /d 0 /f
```

### Disable via Group Policy (Windows Pro/Enterprise)

```powershell
1. Press Windows + R, type: gpedit.msc
2. Navigate to:
   Computer Configuration
   → Administrative Templates
   → Network
   → DNS Client
3. Enable "Turn off multicast name resolution"
4. Click Apply
```

### Verify LLMNR is Disabled

```cmd
netstat -ano | find ":5355"
```

If no output, LLMNR is successfully disabled.

---

## 5. Disable mDNS (Multicast DNS)

### Why This Matters

mDNS is enabled by default in Windows Server 2019/2022 and Windows 10/11, broadcasting device information on UDP/5353.

### Disable mDNS via Registry

```powershell
# Run as Administrator
reg add "HKLM\SYSTEM\CurrentControlSet\Services\DNScache\Parameters" /v EnableMDNS /t REG_DWORD /d 0 /f

# Restart DNS Client service
Restart-Service Dnscache
```

### Verify mDNS is Disabled

```cmd
netstat -ano | find ":5353"
```

If no output, mDNS is successfully disabled.

### Uninstall Bonjour (If Installed)

If you have Apple software installed (iTunes, QuickTime), it may have installed Bonjour:

```powershell
1. Press Windows + R, type: appwiz.cpl
2. Find "Bonjour" in the list
3. Right-click → Uninstall
4. Restart your computer
```

---

## 6. Verification & Testing

### Check Current Network Broadcasts

```powershell
# Run as Administrator - Check all broadcast protocols
netstat -ano | findstr "137 138 139 5353 5355"

# Should return no results if all protocols disabled
```

### Check Computer Name Broadcast

```powershell
# View computer name
hostname

# View network configuration
ipconfig /all

# Look for "Host Name" - should be generic
```

### Monitor Network Traffic (Advanced)

Use Wireshark or Microsoft Network Monitor to verify no hostname leaks:

```powershell
# Install Wireshark, then filter for:
# - udp.port == 137 (NetBIOS)
# - udp.port == 5355 (LLMNR)
# - udp.port == 5353 (mDNS)
```

---

## Complete Configuration Summary

### Registry Commands (Run All at Once)

```powershell
# Run PowerShell as Administrator
# Copy and paste entire block

# Disable LLMNR
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\DNSClient" /v EnableMulticast /t REG_DWORD /d 0 /f

# Disable mDNS
reg add "HKLM\SYSTEM\CurrentControlSet\Services\DNScache\Parameters" /v EnableMDNS /t REG_DWORD /d 0 /f

# Disable NetBIOS on all adapters
$regkey = "HKLM:SYSTEM\CurrentControlSet\services\NetBT\Parameters\Interfaces"
Get-ChildItem $regkey | ForEach-Object { 
    Set-ItemProperty -Path "$regkey\$($_.PSChildName)" -Name NetbiosOptions -Value 2 -Verbose
}

# Restart DNS Client
Restart-Service Dnscache

Write-Host "Privacy settings applied. Please restart your computer." -ForegroundColor Green
```

---

## Quick Setup Script

Save this as `Windows-WiFi-Privacy.ps1`:

```powershell
#Requires -RunAsAdministrator
# Windows WiFi Privacy Configuration Script

Write-Host "Configuring Windows WiFi Privacy Settings..." -ForegroundColor Cyan

# 1. Disable LLMNR
Write-Host "`n[1/4] Disabling LLMNR..." -ForegroundColor Yellow
try {
    reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\DNSClient" /v EnableMulticast /t REG_DWORD /d 0 /f | Out-Null
    Write-Host "✓ LLMNR disabled" -ForegroundColor Green
} catch {
    Write-Host "✗ Failed to disable LLMNR" -ForegroundColor Red
}

# 2. Disable mDNS
Write-Host "`n[2/4] Disabling mDNS..." -ForegroundColor Yellow
try {
    reg add "HKLM\SYSTEM\CurrentControlSet\Services\DNScache\Parameters" /v EnableMDNS /t REG_DWORD /d 0 /f | Out-Null
    Restart-Service Dnscache -Force
    Write-Host "✓ mDNS disabled" -ForegroundColor Green
} catch {
    Write-Host "✗ Failed to disable mDNS" -ForegroundColor Red
}

# 3. Disable NetBIOS
Write-Host "`n[3/4] Disabling NetBIOS on all adapters..." -ForegroundColor Yellow
try {
    $regkey = "HKLM:SYSTEM\CurrentControlSet\services\NetBT\Parameters\Interfaces"
    Get-ChildItem $regkey | ForEach-Object { 
        Set-ItemProperty -Path "$regkey\$($_.PSChildName)" -Name NetbiosOptions -Value 2 -ErrorAction SilentlyContinue
    }
    Write-Host "✓ NetBIOS disabled" -ForegroundColor Green
} catch {
    Write-Host "✗ Failed to disable NetBIOS" -ForegroundColor Red
}

# 4. Instructions for MAC randomization
Write-Host "`n[4/4] MAC Randomization (Manual Step Required)" -ForegroundColor Yellow
Write-Host "To enable MAC randomization:" -ForegroundColor White
Write-Host "  1. Open Settings > Network & Internet > Wi-Fi" -ForegroundColor Gray
Write-Host "  2. Click on your WiFi connection" -ForegroundColor Gray
Write-Host "  3. Set 'Random hardware addresses' to 'Change daily'" -ForegroundColor Gray

Write-Host "`n✓ Privacy configuration complete!" -ForegroundColor Green
Write-Host "Please restart your computer for all changes to take effect.`n" -ForegroundColor Cyan
```

Run with:

```powershell
# Right-click PowerShell → Run as Administrator
Set-ExecutionPolicy Bypass -Scope Process -Force
.\Windows-WiFi-Privacy.ps1
```

---

## Additional Privacy Layers

### Windows Firewall Rules (Block Broadcasts)

```powershell
# Run as Administrator

# Block outbound NetBIOS
New-NetFirewallRule -DisplayName "Block NetBIOS Out" -Direction Outbound -Protocol UDP -LocalPort 137,138 -Action Block
New-NetFirewallRule -DisplayName "Block NetBIOS TCP Out" -Direction Outbound -Protocol TCP -LocalPort 139 -Action Block

# Block outbound LLMNR
New-NetFirewallRule -DisplayName "Block LLMNR Out" -Direction Outbound -Protocol UDP -LocalPort 5355 -Action Block

# Block outbound mDNS
New-NetFirewallRule -DisplayName "Block mDNS Out" -Direction Outbound -Protocol UDP -LocalPort 5353 -Action Block
```

### VPN (Recommended)

While VPN doesn't hide device identity from the router, it:

- ✅ Encrypts all traffic content
- ✅ Hides browsing activity from network owner
- ✅ Prevents ISP monitoring
- ❌ Router still sees: MAC, connection existence, VPN server IP

---

## What the Router Sees After Configuration

✅ **Hidden:**

- Random MAC address (changes daily or per network)
- Generic computer name
- No NetBIOS broadcasts
- No LLMNR queries
- No mDNS advertisements

❌ **Still Visible:**

- IP address (assigned by router - unavoidable)
- Traffic patterns (timing, packet sizes, protocols)
- Connection duration
- VPN usage (encrypted, but visible)

---

## Troubleshooting

### MAC Randomization Not Available

Some older WiFi adapters don't support randomization:

```powershell
# Check adapter capabilities
Get-NetAdapter | Select-Object Name, InterfaceDescription, MacAddress

# If option not available in Settings, adapter doesn't support it
```

### Network Discovery Broken

If you need to access network shares/printers:

```powershell
# Temporarily enable for home network only
# Disable randomization for home WiFi
# Re-enable NetBIOS only for trusted networks
```

### Connection Issues After Configuration

```powershell
# Reset network settings
netsh winsock reset
netsh int ip reset
ipconfig /release
ipconfig /renew
ipconfig /flushdns

# Restart computer
```

### Revert All Changes

```powershell
# Run as Administrator

# Re-enable LLMNR
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\DNSClient" /v EnableMulticast /t REG_DWORD /d 1 /f

# Re-enable mDNS
reg add "HKLM\SYSTEM\CurrentControlSet\Services\DNScache\Parameters" /v EnableMDNS /t REG_DWORD /d 1 /f

# Re-enable NetBIOS (use DHCP setting)
$regkey = "HKLM:SYSTEM\CurrentControlSet\services\NetBT\Parameters\Interfaces"
Get-ChildItem $regkey | ForEach-Object { 
    Set-ItemProperty -Path "$regkey\$($_.PSChildName)" -Name NetbiosOptions -Value 0
}

# Restart services
Restart-Service Dnscache

Write-Host "Privacy settings reverted. Please restart your computer."
```

---

## Limitations & Considerations

> [!warning] Known Limitations
> 
> - **Corporate Networks**: May require NetBIOS for domain authentication
> - **Network Shares**: Home file/printer sharing may not work with broadcasts disabled
> - **MAC Whitelisting**: Some networks reject randomized MACs
> - **Behavioral Analysis**: Traffic patterns can still correlate devices

### When NOT to Use These Settings

- Corporate/domain environments requiring NetBIOS
- Home network with printer/file sharing needs
- Networks with MAC address whitelisting
- Gaming with UPnP requirements

---

## References & Sources

- Microsoft Learn: [Network Settings](https://learn.microsoft.com/en-us/windows/)
- Pureinfotech: [MAC Address Randomization Windows 11](https://pureinfotech.com/enable-random-hardware-addresses-windows-11/)
- Windows OS Hub: [Disable NetBIOS, LLMNR, mDNS](https://woshub.com/how-to-disable-netbios-over-tcpip-and-llmnr-using-gpo/)

---

## Tags

#windows #privacy #networking #wifi #security #netbios #llmnr #mdns #mac-address

---

**Last Updated:** 2026-01-04  
**Confidence Level:** High  
**Tested On:** Windows 10 21H2+, Windows 11 22H2+