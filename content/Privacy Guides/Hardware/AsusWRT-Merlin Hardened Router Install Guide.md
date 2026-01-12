
> [!info] Document Purpose: 
> 
> Get your ASUS router running AsusWRT-Merlin firmware with secure defaults. 
> 
> **Time required:** 20-30 minutes 
> 
> **Skill level:** Beginner to Intermediate - there might be some complexity if you have never tried a firmware upgrade. If you have, this should be fairly straightforward.

---

## Step 1: Verify Compatibility

Check your router model is supported: https://www.asuswrt-merlin.net/about

Common supported models:

- RT-AX88U, RT-AX86U, RT-AX68U
- RT-AC68U, RT-AC86U, RT-AC5300
- GT-AX6000, GT-AXE16000
- RT-BE88U, RT-BE86U

---

## Step 2: Download Firmware

**Official source:** https://www.asuswrt-merlin.net/download

1. Find your exact router model
2. Download the latest firmware (`.trx` or `.w` file)
3. Note the version number

> [!note] Current Versions (Jan 2026)
> 
> - 3006.102.6 (Most WiFi 6/7 models)
> - 3004.388.11 (Legacy models)

---

## Step 3: Backup Current Settings

1. Log into router: `http://router.asus.com` or `http://192.168.1.1`
2. Go to **Administration > Restore/Save/Upload Setting**
3. Click **Save setting**
4. Save the `.CFG` file to your computer

---

## Step 4: Flash Merlin Firmware

> [!warning] Important Check If your router is on stock firmware version **3.0.0.6.102_37000 or newer**, you need to bypass downgrade protection first.

**Check your version:** Look at the top of the admin page.

**If version 3.0.0.6.102_37000 or newer:**

1. Enable SSH temporarily:
    - **Administration > System > Service**
    - Enable SSH: **Yes**
    - Apply
2. SSH to router: `ssh admin@192.168.1.1`
3. Run: `nvram set DOWNGRADE_CHECK_PASS=1`
4. Type `exit`

**Flash the firmware:**

1. **Reboot router first** (clears RAM)
2. After reboot, log back into admin panel
3. Go to **Administration > Firmware Upgrade**
4. Click **Choose File** and select the Merlin `.trx`/`.w` file
5. Click **Upload**
6. **Wait 3-5 minutes** - Do NOT interrupt power
7. Router will reboot automatically

---

## Step 5: Factory Reset (Recommended)

After first Merlin install, do a factory reset to prevent issues:

1. **Administration > Restore/Save/Upload Setting**
2. Click **Restore** (Initialize all settings)
3. Click **OK** to confirm
4. Router will reboot

---

## Step 6: Initial Configuration

Router will run **Quick Internet Setup (QIS)** wizard:

1. **Internet Connection:** Choose your ISP type (usually "Automatic IP")
2. **WiFi Network Name (SSID):** Choose unique name
3. **WiFi Password:** **12+ characters, complex** (letters, numbers, symbols)
4. **Router Admin Username:** Keep as `admin` or change
5. **Router Admin Password:** **Different from WiFi password, 12+ characters**

> [!warning] Critical
> 
> - Use DIFFERENT passwords for WiFi and admin
> - Never use default passwords
> - Write passwords down somewhere safe

---

## Step 7: Essential Security Settings

### 7.1 Disable Risky Services

**Go to: Administration > System**

```
Enable Telnet: NO
Enable SSH: LAN only (we'll configure this next)
Allow SSH password login: Yes (for now)
SSH Brute Force Protection: YES
```

**Go to: WAN > Internet Connection**

```
Enable UPnP: NO (unless you know you need it)
```

**Go to: USB Application (if you have one)**

```
AiCloud: Disable ALL services
Download Master: OFF (unless needed)
Media Server: OFF (unless needed)
```

**Go to: Firewall > General**

```
Enable Firewall: YES
Enable DoS protection: YES
Respond to ping from WAN: NO
```

### 7.2 WiFi Security

**Go to: Wireless > General**

```
Authentication Method: WPA2-Personal or WPA3-Personal
WPA Encryption: AES
Enable WPS: NO
Hide SSID: NO (doesn't help security, causes problems)
```

### 7.3 DNS Security

**Go to: WAN > Internet Connection > WAN DNS Settings**

```
Connect to DNS Server automatically: NO
DNS Server 1: 1.1.1.1 (Cloudflare)
DNS Server 2: 8.8.8.8 (Google)
```

**Go to: WAN > DNS Privacy Protocol**

```
DNS-over-TLS (DoT): Enable
Select a preset server: Cloudflare or Quad9
```

### 7.4 Guest Network for IoT Devices

**Go to: Wireless > Guest Network**

Set up a separate network for smart home devices, security cameras, etc.

**2.4 GHz Guest Network 1:**

```
Enable Guest Network: Yes
Network Name (SSID): Home-IoT (or whatever you want)
Authentication Method: WPA2-Personal
WPA Encryption: AES
WPA-PSK Password: [Different password than main WiFi]
Access Intranet: DISABLE ⚠️ (this isolates IoT from your computers/phones)
Access Time: Unlimited
```

> [!success] Why This Matters
> 
> - IoT devices (cameras, smart bulbs, thermostats) often have poor security
> - **Access Intranet: DISABLE** prevents compromised IoT device from attacking your computers
> - They still get internet access, just can't see your main network
> - Your phone/laptop on main WiFi can still control IoT devices via cloud apps

**Optional - 5 GHz Guest Network:**

Same settings as above if you have 5 GHz IoT devices (uncommon).

---

## Step 8: Test SSH Access (Optional)

SSH is already configured from Step 7.1. If you want to test it:

**Connect from your computer:**

```bash
ssh admin@192.168.1.1
```

Enter your admin password when prompted.

> [!note] SSH Security
> 
> - SSH only accessible from LAN (not from internet)
> - Brute force protection enabled
> - Use the same strong admin password you set earlier
> 
> **Advanced users:** You can set up SSH key authentication later for password-less login. See the wiki: https://github.com/RMerl/asuswrt-merlin.ng/wiki/SSHD

---

## Step 9: Verify Security

**Go to: Administration > System**

Scroll to **Router Security Assessment** and click **Scan**

Address any **red** warnings shown.

**Expected green status:**

- Strong admin password
- WPS disabled
- Firewall enabled
- Remote access disabled
- AiCloud disabled

---

## Step 10: Backup & Maintenance

### Backup Your Configuration

**Administration > Restore/Save/Upload Setting > Save setting**

Save this file monthly and after changes.

### Update Schedule

Check for updates monthly:

- Visit https://www.asuswrt-merlin.net/changelog
- Follow Steps 4-5 to flash new firmware
- Do NOT factory reset for regular updates

---

## Quick Reference: What You Disabled & Why

|Service|Why Disabled|Impact|
|---|---|---|
|**Telnet**|Unencrypted, insecure|None - use SSH instead|
|**WPS**|Fundamentally broken protocol|Minor - pair devices manually|
|**UPnP**|Auto-opens ports, security risk|Minor - may need manual port forwarding|
|**AiCloud**|Recent critical CVEs|None unless you use it|
|**WAN Remote Access**|Exposes admin to internet|None - use VPN if remote access needed|

---

## Troubleshooting

**Can't access router after changes:**

- Try `http://router.asus.com` or `http://192.168.1.1`
- Make sure you're on router's WiFi or plugged into LAN
- Factory reset: Hold reset button 10 seconds (while powered on)

**Internet not working:**

- Verify WAN cable plugged into WAN port (not LAN port)
- Check Network Map shows "Connected" under Internet Status
- Reboot: Power off modem, power off router, wait 30s, power on modem, wait 2min, power on router

**Can't SSH:**

- Verify you're on the router's network (not cellular/guest WiFi)
- Check SSH is enabled: Administration > System > Enable SSH: LAN only
- Username is `admin` (or whatever you set)

---

## What's Next (Optional)

Once comfortable with basic Merlin:

- Install **amtm** (run `amtm` via SSH) for add-on scripts
- Consider **Skynet** (firewall enhancement) or **Diversion** (ad-blocking)
- Set up VPN server for secure remote access
- Configure guest network for IoT devices

---

## Critical Security Patches (2025-2026)

> [!warning] Known Vulnerabilities
> 
> - **CVE-2025-2492** (Critical): AiCloud RCE - Fixed in 3006.102.3+
> - **CVE-2023-39780** (High): SSH/WAN exploitation - Mitigated by disabling WAN access
> 
> **Always keep firmware updated.**

---

## Resources

- **Official Site:** https://www.asuswrt-merlin.net/
- **Wiki:** https://github.com/RMerl/asuswrt-merlin.ng/wiki
- **Support Forum:** https://www.snbforums.com/forums/asuswrt-merlin.42/
- **Security Advisories:** https://www.asus.com/security-advisory/

---

**Document Version:** 2026-01-04 **Based on:** AsusWRT-Merlin 3006.102.6 / 3004.388.11

> [!success] Setup Complete Your router is now running hardened AsusWRT-Merlin firmware with secure defaults.