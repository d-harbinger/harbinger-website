>[!warning] THIS GUIDE IS CURRENTLY UNDER CONSTRUCTION - CONTINUE AT YOUR OWN RISK

# GrapheneOS Installation Guide: Best Buy to Boot

**Last Verified:** January 2026  
**Official Source:** https://grapheneos.org/install/web  
**Status:** ✅ Verified against official documentation

---

## 📋 Table of Contents

1. [[#Overview]]
2. [[#Part 1 Device Purchase]]
3. [[#Part 2 System Requirements]]
4. [[#Part 3 Pre-Installation Preparation]]
5. [[#Part 4 Computer Preparation]]
6. [[#Part 5 GrapheneOS Installation Web Installer]]
7. [[#Part 6 Post-Installation Configuration]]
8. [[#Part 7 Troubleshooting]]
9. [[#Part 8 Reverting to Stock Android]]

---

## Overview

> [!abstract] What is GrapheneOS? GrapheneOS is a privacy and security-focused mobile operating system based on the Android Open Source Project (AOSP). It provides substantial hardening over standard Android while maintaining compatibility with Android apps.

> [!danger] Critical Prerequisites - READ FIRST
> 
> - ❌ Only Google Pixel devices are officially supported
> - ❌ **MUST** purchase UNLOCKED variant (not carrier-locked)
> - ⚠️ Requires temporary internet connection for initial OEM unlock check
> - 💥 **Installation process WILL ERASE ALL DEVICE DATA**
> - 📱 Unlocking bootloader may void manufacturer warranty

> [!tip] Why This Guide? This guide follows Occam's Razor: start with the simplest method (web installer) and provides the complete chain from cash purchase to working installation. All steps verified against official GrapheneOS documentation as of January 2026.

---

## Part 1: Device Purchase

### 🎯 Recommended Devices (January 2026)

> [!success] Tier 1 - Best Long-term Support **7 years minimum support + hardware memory tagging (ARMv9)**
> 
> - Pixel 9 / 9 Pro / 9 Pro XL / 9 Pro Fold
> - Pixel 10 / 10 Pro / 10 Pro XL _(experimental support)_
> - Pixel 8 / 8a / 8 Pro

> [!check] Tier 2 - Good Support **5 years minimum support**
> 
> - Pixel 7 / 7a / 7 Pro
> - Pixel 6 / 6a / 6 Pro

> [!warning] Not Recommended **Extended support only - approaching end-of-life**
> 
> - Pixel 5a and earlier models
> - These receive minimal updates and will lose support soon

---

### 🏪 Purchase at Best Buy

> [!danger] CRITICAL: Purchase "Unlocked" Variant Only **This is the single most important decision in this entire process.**
> 
> Carrier-locked devices have a permanent restriction that **CANNOT** be removed, even after:
> 
> - Paying off the device in full
> - Completing contract terms
> - Requesting carrier unlock
> 
> **The OEM unlock will be permanently disabled.**

#### Step-by-Step Purchase Process

**1. Locate Unlocked Pixel Devices:**

```
In-store location: Mobile phone section
Look for: Devices with "Unlocked" or "Unlocked for All Carriers" label
Verify: Packaging shows "Unlocked" designation (not carrier logo)
```

> [!tip] What to Ask Best Buy Associate "I need an **unlocked** Google Pixel [model], **not** a carrier-specific version. Can you verify this is the unlocked variant that works with all carriers?"

**2. Payment Method:**

- ✅ Cash accepted at register
- ✅ Request receipt (needed for warranty)
- ✅ No account/phone number required for cash
- ✅ No personal information collection

**3. What to Avoid:**

> [!danger] Red Flags - DO NOT PURCHASE
> 
> - ❌ ANY carrier branding on box (Verizon, AT&T, T-Mobile, etc.)
> - ❌ Devices labeled "Activate Today" with carrier logos
> - ❌ "Locked to [Carrier]" anywhere on packaging
> - ❌ Contract/installment plan offerings (even paying full price)
> - ❌ Employee saying "we can unlock it later" (they cannot)

---

### 🔍 Why Unlocked Matters (Technical Explanation)

> [!note] Technical Details **Carrier variants** have a carrier ID permanently flashed to the `persist` partition during manufacturing. This carrier ID:
> 
> - Activates carrier-specific configuration in stock Android OS
> - **Disables OEM unlocking** capability at hardware level
> - **Disables bootloader unlocking** capability
> - Cannot be removed through software
> - Persists even after factory reset
> - Blocks GrapheneOS installation permanently
> 
> **Unlocked variants** have carrier ID set to zero, allowing full control.

> [!important] Verification Before Leaving Store If possible, ask to verify the device before purchase:
> 
> 1. Open box and boot device
> 2. Complete minimal setup
> 3. Check Settings → Developer Options → "OEM unlocking"
> 4. If greyed out permanently = carrier locked
> 5. Return immediately if locked

---

## Part 2: System Requirements

### 💻 Computer Requirements

> [!check] Hardware Requirements
> 
> - **RAM:** Minimum 2GB free
> - **Storage:** Minimum 32GB free space
> - **USB Port:** USB-C preferred, USB-A acceptable with quality cable
> - **Connection:** Direct port (no hubs)

#### Supported Operating Systems

> [!success] Officially Supported
> 
> - ✅ Windows 10 / 11
> - ✅ macOS (recent versions)
> - ✅ Linux (most distributions)
> - ✅ ChromeOS
> - ✅ Android tablets (web installer only)

> [!warning] Not Recommended
> 
> - ❌ **Virtual Machines** (USB passthrough unreliable)
> - ❌ **USB hubs** (use direct rear/laptop ports only)
> - ❌ **Incognito/private browsing mode** (causes connection issues)
> - ❌ **Outdated OS versions** (update before proceeding)

#### Supported Browsers (Web Installer)

```
✅ Google Chrome (recommended)
✅ Microsoft Edge
✅ Chromium
✅ Brave
✅ Vanadium (on GrapheneOS/ChromeOS)
```

---

### 📦 Required Items Checklist

> [!todo] Before Starting Installation
> 
> - [ ] Compatible Google Pixel device (unlocked)
> - [ ] High-quality USB-C to USB-C cable (or USB-C to USB-A)
> - [ ] Computer meeting above requirements
> - [ ] Internet connection (for computer AND initial phone setup)
> - [ ] 30-60 minutes of uninterrupted time
> - [ ] Power outlet (keep devices charged)

> [!tip] Cable Quality Matters **Poor quality cables are the #1 cause of installation failures.**
> 
> Use:
> 
> - ✅ Cable that came with Pixel (best option)
> - ✅ High-quality certified USB-C cable
> - ✅ Cable rated for data transfer (not just charging)
> 
> Avoid:
> 
> - ❌ Cheap/unknown brand cables
> - ❌ Damaged/frayed cables
> - ❌ Charge-only cables

---

## Part 3: Pre-Installation Preparation

> [!danger] STOP - Read This First You are about to temporarily boot stock Android and connect to the internet. This seems contradictory for a privacy-focused installation, but it's **required** by Google's security mechanism.
> 
> **Why required:** Google prevents stolen/financed phones from being unlocked. The device must "phone home" once to verify it's not carrier-locked.
> 
> **Privacy impact:** Minimal - you'll immediately erase and replace the OS. Minimize data shared during this setup.

---

### Step 1: Initial Device Boot (ONE TIME ONLY)

> [!important] This Step Requires Internet Even though you're installing privacy-focused software, you **MUST** connect to internet once. This cannot be avoided.

**Minimal Setup Process:**

```
1. Power on new Pixel device
2. Select language
3. Connect to Wi-Fi (REQUIRED - cannot skip)
4. Skip/decline all optional services:
   ❌ Skip Google account sign-in
   ❌ Decline Google services
   ❌ Decline location services
   ❌ Skip backup/restore
   ❌ Decline app installations
5. Complete ONLY required steps to reach home screen
```

> [!tip] Privacy During Setup
> 
> - Use fake name if prompted
> - Skip all optional data sharing
> - Don't sign into Google account
> - Don't install apps
> - Don't enter personal information
> - You'll erase all this in 10 minutes anyway

---

### Step 2: Enable Developer Options

> [!check] Enabling Developer Mode
> 
> ```
> Settings → About phone → Tap "Build number" 7 times
> ```
> 
> **Expected result:** Toast message appears: "You are now a developer!"

> [!note] Why This Works This is an intentional Easter egg in Android. Tapping the build number 7 times unlocks developer settings used by app developers and power users.

---

### Step 3: Enable OEM Unlocking

> [!important] Critical Step - OEM Unlocking
> 
> ```
> Settings → System → Developer options → Toggle "OEM unlocking" ON
> ```

> [!danger] If "OEM unlocking" is Greyed Out **This indicates a problem. Possible causes:**
> 
> 1. **No internet connection** (Temporary)
>     - ✅ Connect to Wi-Fi
>     - ✅ Wait 5-10 minutes
>     - ✅ Reboot and check again
> 2. **Server check delay** (Temporary)
>     - ✅ Wait up to 24 hours with internet connected
>     - ✅ Keep Wi-Fi enabled
>     - ✅ Check periodically
> 3. **Carrier-locked device** (PERMANENT - Cannot be fixed)
>     - ❌ Device was sold by carrier
>     - ❌ OEM unlock permanently disabled
>     - ❌ **MUST RETURN DEVICE**
>     - ❌ Purchase unlocked variant instead
> 4. **Pixel 6a specific issue** (See below)

> [!warning] Special Case: Pixel 6a Factory Firmware If you have a **brand new Pixel 6a** straight from factory:
> 
> **Problem:** OEM unlocking won't work with factory firmware
> 
> **Solution:**
> 
> 1. Update to June 2022 release or later (OTA update)
> 2. Settings → System → System update → Check for update
> 3. Install all available updates
> 4. **Factory reset device** (required after update)
> 5. Settings → System → Reset options → Erase all data
> 6. Complete setup again
> 7. Enable Developer Options again
> 8. NOW enable OEM unlocking

---

### Step 4: Update Stock OS (Recommended)

> [!check] Update Before Installing
> 
> ```
> Settings → System → System update → Check for update
> ```

**Why update?**

- ✅ Latest firmware for installation process
- ✅ Better USB connection stability
- ✅ GrapheneOS will flash latest firmware anyway
- ✅ Reduces potential compatibility issues

> [!note] Update Process
> 
> 1. Check for updates
> 2. Download all available updates
> 3. Install updates (device will reboot)
> 4. Repeat until "System is up to date"
> 5. Reboot device one final time

---

## Part 4: Computer Preparation

> [!abstract] Platform-Specific Driver Installation Different operating systems require different driver setups for fastboot communication.

---

### 🪟 Windows 10/11

> [!check] Windows Driver Installation

**Method 1: Windows Update (Recommended)**

1. Boot Pixel into bootloader mode (see Part 5, Step 1)
2. Connect device to PC via USB
3. Open Windows Update:
    
    ```
    Settings → Windows Update
    ```
    
4. Click "Check for updates"
5. Click "View optional updates"
6. Look for: **"LeMobile Android Device"**
    - (This is the Android bootloader driver - name is due to USB ID overlap)
7. Install the driver
8. Wait for installation to complete
9. Device should now be recognized

**Method 2: Manual Driver Installation (Alternative)**

1. Download latest Pixel USB drivers from Google
2. Open Device Manager (`Win + X` → Device Manager)
3. Find device with yellow warning icon
4. Right-click → Update driver
5. Browse to downloaded driver location
6. Install driver

> [!warning] Driver Issues If device not recognized:
> 
> - Try different USB port (rear ports on desktop)
> - Disable USB 3.0 in BIOS (use USB 2.0 port)
> - Try different cable
> - Restart computer
> - Uninstall and reinstall driver

---

### 🐧 Linux (Debian/Ubuntu)

> [!check] Debian/Ubuntu Setup

**Install required package:**

```bash
sudo apt update
sudo apt install android-sdk-platform-tools-common
```

**Verify installation:**

```bash
fastboot --version
```

> [!tip] udev Rules The `android-sdk-platform-tools-common` package includes udev rules allowing non-root USB access to Android devices.
> 
> If you installed fastboot differently, you may need to manually add udev rules.

---

### 🐧 Linux (Arch)

> [!check] Arch Linux Setup

**Install required package:**

```bash
sudo pacman -S android-udev
```

**Reload udev rules:**

```bash
sudo udevadm control --reload-rules
sudo udevadm trigger
```

---

### 🍎 macOS

> [!success] macOS - No Additional Setup Required macOS includes native support for Android fastboot devices.
> 
> **Requirements:**
> 
> - ✅ Keep system updated
> - ✅ May need to allow USB connection in System Preferences
> - ✅ Use high-quality cable

> [!tip] macOS USB Permission When device connects, macOS may ask to trust the device. Click "Trust" if prompted.

---

## Part 5: GrapheneOS Installation (Web Installer)

> [!abstract] Web Installer Overview The web installer uses WebUSB technology to communicate directly with your device through the browser. It's the easiest and recommended method for most users.
> 
> **Estimated time:** 15-30 minutes

---

### Step 1: Boot into Bootloader Mode

> [!important] Entering Fastboot Mode

**Process:**

1. **Power off device completely**
    
    - Hold power button
    - Tap "Power off"
    - Wait for complete shutdown
2. **Enter bootloader:**
    
    - Press and hold: **Volume Down + Power Button**
    - Hold BOTH buttons simultaneously
    - Keep holding for ~3-5 seconds
3. **Expected result:**
    
    - Screen shows "Fastboot Mode" text
    - Red triangle with Android logo
    - Device model and variant info
    - Serial number
4. **Release buttons**
    

> [!tip] Visual Confirmation You should see a screen that looks like this:
> 
> ```
> ━━━━━━━━━━━━━━━━━━
>      ⚠️
>     /  \
>    /    \
>   ────────
>   FASTBOOT MODE
> 
>   PRODUCT_NAME - [model]
>   VARIANT - [variant]
>   SERIAL NUMBER - [serial]
> ━━━━━━━━━━━━━━━━━━
> ```

---

### Step 2: Connect Device to Computer

> [!check] USB Connection

**Process:**

1. Use high-quality USB-C cable
2. Connect Pixel to computer
3. Connect directly to rear port (desktops) or main port (laptops)
4. Avoid USB hubs

**Expected behavior:**

- Windows: Device should be recognized (driver installed in Part 4)
- macOS: Device recognized automatically
- Linux: udev rules allow access (installed in Part 4)

> [!warning] Connection Troubleshooting **Device not recognized?**
> 
> Try in order:
> 
> 1. Different USB port (rear ports preferred on desktop)
> 2. Different USB cable
> 3. Restart computer
> 4. Verify driver installation (Windows)
> 5. Verify udev rules (Linux)
> 6. Try USB 2.0 port instead of 3.0
> 7. Disable USB selective suspend (Windows)

---

### Step 3: Access Web Installer

> [!important] Official Web Installer **Open browser and navigate to:**
> 
> ```
> https://grapheneos.org/install/web
> ```

> [!danger] Security Notice **ONLY** use the official installer at `grapheneos.org`
> 
> ❌ Do NOT use:
> 
> - Third-party installation guides
> - Unofficial tools
> - Modified installers
> - YouTube tutorial tools
> 
> ✅ Verify URL is exactly: `https://grapheneos.org/install/web`

**Expected behavior:**

- Webpage loads
- Browser detects device in fastboot mode
- Interface shows device detected
- "Unlock bootloader" button becomes active

> [!tip] If Device Not Detected
> 
> - Ensure device is in fastboot mode (see Step 1)
> - Try different browser (Chrome/Edge recommended)
> - Disable browser extensions temporarily
> - Don't use incognito/private mode
> - Refresh the page
> - Check USB connection

---

### Step 4: Unlock Bootloader

> [!danger] WARNING - Data Will Be Erased Unlocking the bootloader will **ERASE ALL DATA** on the device.
> 
> This includes:
> 
> - All apps
> - All settings
> - All files
> - All accounts
> - Everything
> 
> **This is a security feature and cannot be avoided.**

**Process:**

1. On webpage: Click **"Unlock bootloader"** button
    
2. On Pixel device:
    
    - Screen shows unlocking warning
    - Use **Volume buttons** to navigate
    - Select "**Unlock the bootloader**" option
    - Press **Power button** to confirm
3. Device displays warning:
    
    ```
    If you unlock the bootloader, you will be able to
    install custom operating system software on this phone.
    
    A custom OS is not subject to the same testing as the
    original OS, and can cause your phone and installed
    applications to stop working properly.
    
    To prevent unauthorized access to your personal data,
    unlocking the bootloader will also delete all personal
    data from your phone (a "factory data reset").
    ```
    
4. Confirm unlocking
    

**Expected result:**

- Device erases all data
- Bootloader unlocks
- Device remains in fastboot mode
- Web interface confirms unlock

> [!success] Bootloader Now Unlocked The bootloader is now unlocked. This allows installation of custom operating systems like GrapheneOS.

---

### Step 5: Download GrapheneOS

> [!check] Downloading Factory Images

**Process:**

1. On webpage: Click **"Download release"** button
    
2. Select your specific Pixel model from dropdown:
    
    ```
    Example: "Pixel 9 Pro (comet)"
    ```
    
3. Download begins automatically
    

**File size:** Varies by device, typically 1-2GB

**Download time:** Depends on internet speed (5-15 minutes typical)

> [!tip] Verify Download
> 
> - Monitor download progress
> - Ensure download completes fully
> - Check browser downloads folder if needed
> - Don't proceed until download is 100% complete

> [!note] What's Being Downloaded? The factory image contains:
> 
> - GrapheneOS system image
> - Bootloader firmware
> - Radio firmware
> - Boot image
> - Vendor image
> - All necessary partitions

---

### Step 6: Flash GrapheneOS

> [!important] Critical Step - Flashing OS

**Process:**

1. On webpage: Click **"Flash release"** button
    
2. Web installer begins flashing process:
    
    ```
    Progress stages:
    ├── Erasing partitions
    ├── Writing bootloader
    ├── Writing radio firmware  
    ├── Writing system partitions
    ├── Writing boot image
    ├── Writing vendor image
    └── Verifying written data
    ```
    
3. **DO NOT DISCONNECT DEVICE**
    

**Duration:** 5-15 minutes (varies by device)

> [!danger] Critical Warnings During Flash
> 
> - ❌ **DO NOT** disconnect USB cable
> - ❌ **DO NOT** close browser
> - ❌ **DO NOT** power off device
> - ❌ **DO NOT** interrupt the process
> - ❌ **DO NOT** touch device buttons
> 
> Interrupting flash can **brick your device**.

**Monitor progress:**

- Web interface shows progress bar
- Device screen shows flash activity
- Status messages update in real-time

> [!success] Flash Complete When flashing completes:
> 
> - Web interface shows "Flash complete"
> - Device remains in fastboot mode
> - Ready for next step (locking bootloader)

---

### Step 7: Lock Bootloader (CRITICAL FOR SECURITY)

> [!danger] CRITICAL - Do Not Skip This Step **Locking the bootloader is ESSENTIAL for security.**
> 
> Without a locked bootloader:
> 
> - ❌ Verified boot doesn't work
> - ❌ Anyone with physical access can modify OS
> - ❌ Device is vulnerable to evil maid attacks
> - ❌ GrapheneOS security features degraded
> 
> **YOU MUST LOCK THE BOOTLOADER.**

**Process:**

1. On webpage: Click **"Lock bootloader"** button
    
2. On Pixel device:
    
    - Use **Volume buttons** to navigate
    - Select "**Lock the bootloader**"
    - Press **Power button** to confirm
3. Device locks bootloader
    

**Expected result:**

- Bootloader locked
- Verified boot enabled
- Device remains in fastboot mode
- Web interface confirms lock

> [!success] Bootloader Locked ✅ Bootloader is now locked ✅ Verified boot active ✅ GrapheneOS cannot be tampered with ✅ Full security model in effect

---

### Step 8: Reboot and Complete Installation

> [!check] Final Reboot

**Process:**

1. Device automatically reboots (or manually reboot from fastboot menu)
    
2. First boot sequence:
    
    - GrapheneOS boot animation appears
    - First boot takes 2-5 minutes (longer than normal boots)
    - Device initializes all partitions
    - Sets up encrypted storage
3. Setup wizard appears:
    
    - GrapheneOS welcome screen
    - Language selection
    - Timezone configuration
    - Security setup

> [!success] Installation Complete! 🎉 **Congratulations!** GrapheneOS is now installed.
> 
> Your device is running a privacy and security-focused operating system with:
> 
> - ✅ Hardened security model
> - ✅ Verified boot protection
> - ✅ No Google services by default
> - ✅ Enhanced app sandboxing
> - ✅ Privacy-focused defaults

---

## Part 6: Post-Installation Configuration

> [!abstract] Securing Your Installation While GrapheneOS is installed, you should complete these critical post-installation steps to maximize security.

---

### Step 1: Disable OEM Unlocking (CRITICAL)

> [!danger] IMPORTANT FOR SECURITY **You MUST disable OEM unlocking after installation.**
> 
> Why this matters:
> 
> - Prevents unauthorized bootloader unlocking if device stolen
> - Completes the security model
> - Required for full verified boot protection
> - Standard security practice

**Process:**

```
Settings → System → Developer options → Toggle "OEM unlocking" OFF
```

> [!success] Security Checkpoint After disabling OEM unlocking:
> 
> - ✅ Bootloader cannot be unlocked without factory reset
> - ✅ Thief cannot install malicious OS
> - ✅ Data protected even if device stolen
> - ✅ Full hardware-backed security active

---

### Step 2: Initial Setup Wizard

> [!check] Configure Basic Settings

**Complete these steps:**

1. **Select Language**
    
    - Choose your preferred language
2. **Set Timezone**
    
    - Select your timezone
    - Enable/disable automatic timezone
3. **Create Device PIN/Password**
    
    ```
    Required security feature
    Minimum 6 digits recommended
    Stronger = better (passphrase recommended)
    ```
    
4. **Configure Lockscreen**
    
    - Set lock timeout
    - Configure PIN scrambling (optional but recommended)
    - Set up fingerprint (optional)
5. **Skip Google Account** (unless needed)
    
    - You can add sandboxed Google Play later if needed
    - GrapheneOS doesn't require Google account

> [!tip] PIN Scrambling GrapheneOS offers PIN scrambling:
> 
> - Randomizes number pad layout on lockscreen
> - Prevents shoulder surfing
> - Hardens against observation attacks
> 
> Enable in: Settings → Security → PIN scrambling

> [!note] Duress Password GrapheneOS supports a "duress password":
> 
> - Special PIN/password that wipes device when entered
> - Use if forced to unlock under duress
> - Configure in: Settings → Security → Duress password

---

### Step 3: Verify Installation

> [!important] Verify Authenticity

**Method 1: Check Build Number**

```
Settings → About phone → Build number
```

**Should show:**

```
Format: 2025XXXXXX (date-based build number)
Example: 2025121701
Followed by: Device codename
```

> [!success] Valid Build Number If you see a GrapheneOS build number with recent date, installation is correct.

**Method 2: Use Auditor App (Recommended)**

> [!check] Cryptographic Verification

The Auditor app provides cryptographic proof your installation is genuine:

1. Open preinstalled **"Apps"** application
2. Find **"Auditor"**
3. Tap **"Install"**
4. Open Auditor app
5. Follow attestation instructions
6. App verifies:
    - ✅ GrapheneOS authenticity
    - ✅ Bootloader lock status
    - ✅ Verified boot state
    - ✅ No tampering occurred

> [!note] Auditor Details Auditor uses Android's hardware attestation:
> 
> - Cryptographic signatures from Titan M chip
> - Verifies entire boot chain
> - Confirms OS integrity
> - Cannot be spoofed

---

### Step 4: Install Sandboxed Google Play (Optional)

> [!question] Do You Need Google Play?

**Install if you need:**

- Banking apps
- Apps requiring Google Play Services
- Apps requiring SafetyNet/Play Integrity
- Google Play Store access

**Skip if:**

- Using only F-Droid apps
- Using Aurora Store for Play Store apps
- Don't need Google services
- Want maximum privacy

> [!check] Installation Process

**If needed:**

1. Open preinstalled **"Apps"** application
    
2. Find and install (in order):
    
    ```
    1. Google Play Services
    2. Google Services Framework
    3. Google Play Store
    ```
    
3. Tap **"Install"** on each
    
4. Wait for installation to complete
    

> [!success] Sandboxed Google Play GrapheneOS runs Google Play Services in a sandbox:
> 
> - ✅ **No system privileges** (runs as user app)
> - ✅ **No special access** to system
> - ✅ **Fully permission-controlled** by you
> - ✅ **Can be uninstalled** anytime
> - ✅ **Network/sensors gated** per-app
> 
> This is fundamentally different from stock Android where Google Play has system-level privileges.

> [!tip] Per-Profile Installation You can install sandboxed Google Play in:
> 
> - Main profile (all apps access)
> - Specific user profiles only
> - Work profile only
> 
> This compartmentalizes Google services.

---

### Step 5: Configure Privacy & Security Settings

> [!important] Recommended Hardening

**Privacy Settings:**

```
Settings → Privacy:
├── Location
│   └── Disable when not needed (per-app control)
├── Camera  
│   └── Review app permissions
├── Microphone
│   └── Review app permissions
└── Sensors
    └── Configure per-app sensor access
```

**Security Settings:**

> [!check] Essential Security Features

**Auto-Reboot:**

```
Settings → Security → Auto reboot
Recommended: 12-24 hours
```

**Why this matters:**

- Rebooting locks device into BFU (Before First Unlock) state
- Encryption keys not in memory
- Harder to extract data
- Protects against forensic tools

**Network Permission:**

```
Settings → Apps → [App] → Network permission
```

- GrapheneOS allows blocking internet per-app
- Apps function offline but can't send data

**Sensors Permission:**

```
Settings → Apps → [App] → Sensors permission  
```

- Block access to accelerometer, gyroscope, etc.
- Prevents motion-based fingerprinting
- Hardens privacy

> [!tip] Storage & Contact Scopes

GrapheneOS introduces granular controls:

**Storage Scopes:**

```
Settings → Apps → [App] → Storage Scopes
```

- Select specific files/folders app can access
- App doesn't see entire storage
- Prevents data harvesting

**Contact Scopes:**

```
Settings → Apps → [App] → Contact Scopes
```

- Select specific contacts app can access
- App doesn't see full contact list
- Protects contact privacy

> [!example] Use Case Messaging app only needs access to contacts you message, not your entire address book. Grant access to specific contacts only.

---

### Step 6: Additional Hardening (Advanced)

> [!note] Advanced Options

**Exploit Protection:**

```
Settings → Security → Exploit protection
└── Enable hardened malloc (on by default)
```

**Network Features:**

```
Settings → Network & internet:
├── Wi-Fi Privacy
│   └── Randomize MAC per network (enabled by default)
├── Private DNS
│   └── Configure DNS-over-TLS
└── VPN
    └── Configure always-on VPN (optional)
```

**Automatic Features:**

```
Settings → Security:
├── Auto reboot (configure timer)
├── Auto Wi-Fi disable (when not used)
├── Auto Bluetooth disable (when not used)
└── USB-C port disable (lockscreen option)
```

> [!success] Complete Hardening With these settings configured:
> 
> - ✅ Maximum privacy protection
> - ✅ Reduced attack surface
> - ✅ Granular app control
> - ✅ Automatic security measures
> - ✅ Full control over device

---

## Part 7: Troubleshooting

> [!bug] Common Issues & Solutions

---

### Issue 1: "OEM Unlocking" Greyed Out

> [!warning] Diagnosis Tree

```
"OEM unlocking" toggle is greyed out
│
├─ Device has internet connection?
│  ├─ NO → Connect to Wi-Fi, wait 10 minutes, check again
│  └─ YES → Continue
│
├─ Waited 24+ hours with internet?
│  ├─ NO → Wait longer, Google server may be delayed
│  └─ YES → Continue
│
├─ Pixel 6a with factory firmware?
│  ├─ YES → Update to June 2022+, factory reset, try again
│  └─ NO → Continue
│
└─ Still greyed out after all above?
   └─ Device is CARRIER LOCKED (permanent)
      └─ MUST RETURN and purchase unlocked variant
```

**Resolution Steps:**

> [!check] Try These in Order

**1. Internet Connection:**

- Connect to Wi-Fi
- Verify internet works (test in browser)
- Reboot device
- Check OEM unlocking again
- Wait 10-15 minutes

**2. Server Delay:**

- Keep device connected to internet
- Wait up to 24 hours
- Check periodically
- Google servers may be slow to respond

**3. Pixel 6a Specific:**

```
Settings → System → System update → Check for update
Install ALL updates
Factory reset device
Setup again
Enable Developer Options
Try OEM unlocking
```

**4. Carrier Lock (Unfixable):**

> [!danger] If Still Greyed After All Above **Device is carrier-locked. This CANNOT be fixed.**
> 
> Actions:
> 
> 1. ❌ Return device to Best Buy immediately
> 2. ✅ Purchase UNLOCKED variant
> 3. ✅ Verify unlocked label on new device
> 4. ✅ Test OEM unlocking before leaving store

---

### Issue 2: Device Not Detected in Fastboot

> [!bug] Connection Issues

**Symptoms:**

- Web installer doesn't detect device
- "No device detected" message
- Device in fastboot but computer doesn't see it

**Resolution:**

> [!check] Systematic Troubleshooting

**1. Verify Fastboot Mode:**

```
Device screen should show:
"FASTBOOT MODE"
Not: "Start" or normal Android
```

**2. Cable/Port Issues:**

- Try different USB port (rear ports on desktop)
- Try different USB cable (use Pixel's original cable)
- Avoid USB hubs completely
- Connect directly to motherboard port

**3. Driver Issues (Windows):**

```
Device Manager → Other devices
Look for: Device with yellow warning icon

Solution:
Windows Update → View optional updates
Install: "LeMobile Android Device"
```

**4. udev Rules (Linux):**

```bash
# Verify package installed
dpkg -l | grep android-sdk-platform-tools-common

# If not installed:
sudo apt install android-sdk-platform-tools-common

# Reload udev
sudo udevadm control --reload-rules
sudo udevadm trigger
```

**5. USB Port Type:**

- Try USB 2.0 port instead of 3.0
- Some USB 3.0 ports have compatibility issues
- Rear ports more reliable than front panels

---

### Issue 3: Web Installer Not Detecting Device

> [!bug] Browser/Connection Issues

**Symptoms:**

- Device in fastboot
- Computer sees device
- Web installer shows "No device detected"

**Resolution:**

> [!check] Browser-Specific Fixes

**1. Browser Issues:**

- Try different browser (Chrome/Edge recommended)
- Clear browser cache
- Disable ALL browser extensions
- Don't use incognito/private mode
- Update browser to latest version

**2. Page Issues:**

- Hard refresh page (Ctrl+F5 / Cmd+Shift+R)
- Close and reopen browser completely
- Navigate directly to URL (don't use bookmark)
- Check URL is exactly: `https://grapheneos.org/install/web`

**3. WebUSB Permissions:**

- Browser may ask to connect to device
- Click "Allow" or "Connect"
- Check browser site permissions
- Ensure WebUSB not blocked

**4. Computer Issues:**

- Restart computer
- Try different computer if available
- Update OS to latest version

---

### Issue 4: Flash Process Fails/Errors

> [!bug] Flashing Errors

**Symptoms:**

- Flash starts but fails partway through
- Error messages during flash
- Flash hangs/freezes
- "FAILED" messages

**Resolution:**

> [!check] Comprehensive Fix Process

**1. Storage Space:**

```
Verify computer has:
├─ 32GB+ free storage
└─ 2GB+ free RAM

Check:
Windows: This PC → C: drive properties
macOS: About This Mac → Storage
Linux: df -h
```

**2. Power/Connection:**

- Ensure device has >50% battery
- Keep device connected to power during flash
- Don't move/disturb USB cable during flash
- Ensure stable USB connection

**3. Clean Retry:**

- Completely close browser
- Disconnect device
- Reboot device to fastboot mode
- Reboot computer
- Start process fresh from Step 1

**4. Alternative Cable/Port:**

- Use different USB cable
- Use different USB port
- Try USB 2.0 port if using 3.0
- Avoid extension cables/adapters

**5. Download Issues:**

- Re-download factory image
- Verify download completed fully
- Check file size matches expected
- Clear browser download cache

**6. Last Resort:**

- Try different computer
- Try command-line installation method
- Ask for help in GrapheneOS chat

---

### Issue 5: Boot Loop After Installation

> [!bug] Device Won't Boot Properly

**Symptoms:**

- Device reboots repeatedly
- Stuck on boot animation
- Never reaches setup wizard
- Continuous restart

**Resolution:**

> [!check] Boot Issues Fix

**1. Verify Bootloader Lock:**

```
Boot to fastboot mode:
Volume Down + Power

Check screen shows:
"LOCK STATE: locked"

If shows "unlocked":
└─ Use web installer → Lock bootloader again
```

**2. Force Relock:**

- Boot to fastboot mode
- Connect to web installer
- Click "Lock bootloader"
- Confirm lock
- Reboot

**3. Reflash OS:**

- Boot to fastboot mode
- Unlock bootloader
- Flash GrapheneOS again (full process)
- Lock bootloader
- Reboot

**4. Verify Correct Image:**

- Ensure you selected correct device model
- Pixel 9 Pro ≠ Pixel 9
- Each model has specific image
- Re-download if unsure

---

### Issue 6: Specific App Compatibility

> [!bug] Apps Not Working

**Symptoms:**

- Banking app won't open
- "Device not certified" errors
- Apps detecting "rooted" device
- SafetyNet/Play Integrity failures

**Resolution:**

> [!check] App Compatibility Fixes

**1. Install Sandboxed Google Play:**

```
Apps → Install:
├─ Google Play Services
├─ Google Services Framework  
└─ Google Play Store
```

**2. Play Integrity:**

> [!note] Current Status
> 
> - Basic integrity: Usually passes
> - Device integrity: May fail on some apps
> - Strong integrity: Usually fails
> 
> GrapheneOS team working on compatibility layer

**3. Banking Apps:**

- Install sandboxed Google Play first
- Update all Google Play components
- Clear app cache/data
- Reinstall app
- Some banks permanently block custom ROMs

**4. Workarounds:**

- Use web version of app/service
- Contact app developer (some whitelist GrapheneOS)
- Use alternative apps
- Accept some apps won't work

---

## Part 8: Reverting to Stock Android

> [!info] Returning to Stock OS

If you need to return to stock Google Android (e.g., for warranty, resale, or personal preference):

---

### Reversion Process

> [!check] Step-by-Step Reversion

**Step 1: Remove Non-Stock Key**

1. Navigate to: https://grapheneos.org/install/web
2. Scroll to bottom of page
3. Boot device to fastboot mode
4. Connect to computer
5. Click **"Remove the non-stock key"** button
6. Follow prompts

**Step 2: Flash Stock Firmware**

1. Navigate to: https://flash.android.com
2. Connect device (should be in fastboot)
3. Select your Pixel model
4. Choose firmware version:
    - Latest stable
    - Or specific version
5. Click "Install"
6. Wait for flash to complete

**Step 3: Lock Bootloader**

1. Still at flash.android.com
2. Follow prompts to lock bootloader
3. Confirm lock
4. Device will reboot to stock Android

> [!success] Reverted Successfully Device is now running stock Google Android:
> 
> - ✅ Factory firmware restored
> - ✅ Bootloader locked
> - ✅ All GrapheneOS removed
> - ✅ Google services active
> - ✅ Ready for normal use/resale

> [!note] Why Revert? Common reasons:
> 
> - Warranty claim requiring stock OS
> - Selling/trading device
> - App compatibility issues
> - Preference for stock Android
> - Testing/comparison

---

## Part 9: Key Security Reminders

> [!important] Post-Installation Security Checklist

### Critical Security Items

> [!check] Verify These Are Complete

- [ ] **OEM unlocking is DISABLED**
    
    ```
    Settings → System → Developer options → OEM unlocking: OFF
    ```
    
- [ ] **Bootloader is LOCKED**
    
    ```
    Check in fastboot mode: LOCK STATE: locked
    ```
    
- [ ] **Auto-reboot enabled**
    
    ```
    Settings → Security → Auto reboot: 12-24 hours
    ```
    
- [ ] **Strong PIN/password set**
    
    ```
    6+ digits minimum, passphrase recommended
    ```
    
- [ ] **App permissions reviewed**
    
    ```
    Settings → Privacy → Permission manager
    ```
    
- [ ] **Installation verified**
    
    ```
    Use Auditor app for cryptographic verification
    ```
    
- [ ] **Storage/Contact Scopes configured**
    
    ```
    Granular access for sensitive apps
    ```
    

---

### Understanding Security Model

> [!note] GrapheneOS Security Architecture

**Hardware-Backed Security:**

```
Titan M Chip (Secure Element)
├─ Stores encryption keys
├─ Verifies boot chain
├─ Hardware attestation
└─ Rollback protection
```

**Verified Boot:**

```
Boot Chain Verification
├─ Bootloader verified by Titan M
├─ OS verified by bootloader
├─ Apps verified by OS
└─ Cryptographic signatures at each level
```

**Sandboxing:**

```
App Sandbox (Hardened)
├─ Stronger than stock Android
├─ Hardware memory tagging (ARMv9)
├─ Hardened malloc
└─ Exploit mitigations
```

> [!success] Full Security Active With bootloader locked and OEM unlocking disabled:
> 
> - ✅ Verified boot protecting boot chain
> - ✅ Titan M chip validating signatures
> - ✅ Encryption keys hardware-protected
> - ✅ Rollback attacks prevented
> - ✅ Evil maid attacks mitigated
> - ✅ Full GrapheneOS security model active

---

## Part 10: Additional Resources

> [!info] Official Documentation

### Primary Resources

**Official GrapheneOS Sites:**

- **Main site:** https://grapheneos.org
- **Installation guide:** https://grapheneos.org/install/web
- **Usage guide:** https://grapheneos.org/usage
- **FAQ:** https://grapheneos.org/faq
- **Features:** https://grapheneos.org/features

**Community Resources:**

- **Matrix chat:** Linked on grapheneos.org
- **Reddit:** r/GrapheneOS
- **Forum:** discuss.grapheneos.org

---

### Device-Specific Information

> [!note] Supported Devices Details

**Current Support (January 2026):**

|Device|Support Status|End of Support|Notes|
|---|---|---|---|
|Pixel 10 Series|Experimental|2032+|7-year minimum|
|Pixel 9 Series|Stable|2031+|7-year minimum, ARMv9|
|Pixel 8 Series|Stable|2030+|7-year minimum, ARMv9|
|Pixel 7 Series|Stable|2027+|5-year minimum|
|Pixel 6 Series|Stable|2026+|5-year minimum|
|Pixel 5a|Extended|2026|End-of-life soon|

**Key Device Features:**

> [!tip] ARMv9 Benefits (Pixel 8/9/10)
> 
> - Hardware memory tagging (MTE)
> - Enhanced exploit protection
> - Better performance
> - Longer support commitment

---

### Getting Help

> [!question] Support Channels

**Before Asking for Help:**

1. Read this entire guide
2. Check official documentation
3. Verify device is unlocked variant
4. Ensure computer meets requirements
5. Try troubleshooting steps above

**Where to Get Help:**

**Official Channels:**

- Matrix chat (fastest response)
- discuss.grapheneos.org
- GitHub issues (bugs only)

**Community Channels:**

- r/GrapheneOS
- XDA forums (community, not official)

**When Asking:**

> [!tip] Provide This Information
> 
> - Device model (exact variant)
> - Computer OS and version
> - Browser used
> - Specific error messages (exact text)
> - What you've already tried
> - Screenshots if relevant

---

## Appendix: Quick Reference

> [!abstract] Essential Commands & Settings

### Quick Settings Paths

```
OEM Unlocking:
Settings → System → Developer options → OEM unlocking

Auto Reboot:
Settings → Security → Auto reboot

App Permissions:
Settings → Privacy → Permission manager

Storage Scopes:
Settings → Apps → [App] → Storage Scopes

Network Permission:
Settings → Apps → [App] → Network permission

PIN Scrambling:
Settings → Security → PIN scrambling

Duress Password:
Settings → Security → Duress password
```

---

### Fastboot Mode Commands

```
Enter Fastboot:
Volume Down + Power (from power off)

Navigate Menu:
Volume Up/Down = Navigate
Power = Select

Common Options:
- Start (boot normally)
- Restart bootloader
- Recovery mode
- Power off
```

---

### Emergency Numbers

> [!warning] If Device Bricked

**Not Actually Bricked If:**

- Device powers on
- Shows fastboot mode
- Computer recognizes in fastboot
- Can reflash OS

**Actually Bricked (Rare):**

- Device completely dead
- No fastboot mode
- No signs of life
- Hardware failure

**Resolution:** Contact Google support or phone repair shop

---

## Document Information

**Version:** 2.0 (Obsidian Edition)  
**Last Updated:** January 4, 2026  
**Verified Against:** GrapheneOS official documentation  
**Confidence Level:** HIGH (all steps verified from primary sources)

**Data Sources:**

- ✅ grapheneos.org (official documentation)
- ✅ GrapheneOS releases page
- ✅ Installation guides (web installer)
- ✅ Community discussions (verification)
- ✅ Best Buy website (device availability)

**Limitations:**

- Cannot verify current Best Buy stock levels
- Carrier policies may vary by region
- Some device-specific issues may not be covered
- App compatibility varies

> [!success] Guide Complete You should now have:
> 
> - ✅ Understanding of entire process
> - ✅ Knowledge to purchase correct device
> - ✅ Ability to install GrapheneOS
> - ✅ Post-installation security hardening
> - ✅ Troubleshooting capabilities
> 
> **Stay safe and privacy-conscious!**