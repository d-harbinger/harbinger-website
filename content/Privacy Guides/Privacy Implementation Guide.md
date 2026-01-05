

_A practical, weekend-by-weekend guide to reclaiming your digital privacy._

---

## Core Philosophy

1. **Zero Data Loss** - Nobody loses access to anything important
2. **Gradual Migration** - Run parallel systems before cutting over
3. **Reversibility** - Can backtrack if something goes wrong
4. **One Weekend Per Project** - Each major section completed in a weekend

---

## PART 1: PREPARATION

**Time Required: 2-3 hours**

### Inventory Checklist

```markdown
□ List all Google services you actively use
□ Count how many accounts use "Sign in with Google"
□ Document all email addresses you own
□ Find all devices logged into your Google account
□ List family members/friends who share Google services with you
□ Check Google Takeout to see what data Google has
□ Note any work/school requirements for Google
□ List all 2FA apps and where they're used
```

### Create Your Migration Notebook

**Document these items (digital note or physical notebook):**

```markdown
□ Current email address(es)
□ Most important accounts (banking, government, healthcare)
□ Subscription services and their renewal dates
□ People who regularly email you
□ Shared calendars or documents
□ Recovery phone numbers and backup emails
□ Any domain names you own
□ Current browser bookmarks you actually use
```

### Time Planning

```markdown
Weekend 1: Password Manager Setup
Weekend 2: Email Foundation
Weekend 3: Browser Switch
Weekend 4: Data Migration
Weekend 5: Communication Migration (Signal)
Weekend 6: Final Cutover
Optional Future: Linux/GrapheneOS exploration
```

### Prepare Recovery Methods

```markdown
□ Write down current Google password in secure location
□ Print Google backup codes
□ Ensure recovery phone number is current
□ Document security questions/answers
□ Have access to recovery email
□ Take screenshots of important Google settings
```

---

## PART 2: PASSWORD MANAGER FIRST

**Time Required: 3-4 hours**  
**Tool: Bitwarden**

### Why This Comes First

- You'll generate dozens of new passwords during migration
- Need secure storage before changing anything
- Creates foundation for all other security improvements

### Step 1: Create Bitwarden Account

```markdown
□ Go to bitwarden.com
□ Click "Create Account"
□ Use your most secure email (keep Google for now)
□ CREATE MASTER PASSWORD:
  - Use passphrase: 5+ random words
  - Example: "correct-horse-battery-staple-sunrise"
  - Write it down on paper (temporarily)
  - Store paper in secure location
□ Verify email address
```

### Step 2: Install Bitwarden

```markdown
□ Mobile app (iOS/Android)
□ Desktop application
□ Desktop browser extension (Chrome/Firefox/Safari)
□ Log into each frequently used device with master password
□ Enable biometric unlock on mobile (after setup)
```

### Step 3: Import Existing Passwords

```markdown
From Chrome:
□ Settings → Passwords → Three dots → Export passwords
□ Save CSV file
□ Bitwarden Vault → File/Settings → Import Data/VaultOptions
□ Select your password export file
□ Upload
□ PERMANENTLY DELETE the CSV file immediately after

From Firefox:
□ about:logins → Three dots → Export Logins
□ Follow same import process
□ PERMANENTLY DELETE the CSV file

From Safari:
□ File → Export → Passwords
□ Follow same import process
□ PERMANENTLY DELETE the CSV file
```

### Step 4: The Relearning Period

```markdown
Week 1-2 Strategy:
□ Keep browser passwords temporarily
□ When logging in, use Bitwarden instead of browser
□ Add any missing passwords to Bitwarden
□ Get comfortable with the workflow
□ Practice using password generator

After 2 weeks, or when comfortable:
□ Disable browser password saving
□ Double check that you have backup access to your Bitwarden Vault
□ Clear saved browser passwords
□ Commit to Bitwarden only
```

### Step 5: Critical Account Security Upgrade

```markdown
Update these first (generate new passwords):
□ Primary email account
□ Banking/financial accounts
□ Government services (IRS, SSA, etc.)
□ Healthcare portals
□ Work/school accounts
□ Social media (if keeping)
□ Shopping accounts with saved payment methods
```

### Bitwarden Best Practices

```markdown
□ Enable 2FA on Bitwarden itself
□ Set auto-lock timeout (5-15 minutes)
□ Create folders for organization:
  - Financial
  - Shopping
  - Social
  - Work
  - Temporary
□ Use secure notes for:
  - WiFi passwords
  - Security questions
  - Recovery codes
  - Software licenses
□ Regular backups: Export vault monthly (encrypted)
	*This obviously comes with its own risks and advantages. Make your best judgement on how you're using your password manager and where your data is being stored if this is a good use case for you.*
```

---

## PART 3: EMAIL TRANSITION

**Time Required: 4-5 hours initially, then ongoing**  
**Tool: ProtonMail**

### Step 1: Choose Your Proton Plan

```markdown
□ Start with Proton Free to test
□ Consider Mail Plus ($3.99/month) for:
  - Custom email domain
  - More storage (15GB)
  - More addresses (10)
  - Email filters and folders
□ Consider Proton Unlimited ($9.99/month) for:
  - 500GB storage
  - Proton VPN
  - Proton Drive
  - Proton Calendar
```

### Step 2: Create Your Proton Account

```markdown
Username Strategy:

□ Option 1: FirstnameLastname
□ Option 2: Professional variant
□ Option 3: Completely new identity
□ Avoid: Numbers, birth years, old usernames

*Note: I reccomend all of the below. Anonymity is not the only goal here, and you can integrate multiple emails into one account. I suggest starting with firstname.lastname@proton.me and then creating multiple alias as you feel comfortable doing or as needed.*

Setup Process:
□ Go to proton.me/mail
□ Create account with chosen username
□ Use Bitwarden to generate strong password
□ Save immediately in Bitwarden
□ Complete verification (email or captcha)
□ SAVE RECOVERY PHRASE (keep it secret, keep it safe) in Bitwarden secure note
```

### Step 3: Configure Gmail Forwarding

```markdown
Keep Gmail Active While Transitioning:
□ Gmail Settings → Forwarding and POP/IMAP
□ Add forwarding address (your new Proton email)
□ Verify forwarding via code sent to Proton
□ Choose "Forward a copy and delete Gmail's copy"
□ This ensures you don't miss anything but everything in Gmail is deleted
```

![[Pasted image 20251119143227.png]]

![[Pasted image 20251119143314.png]]

### Step 4: Import Contacts

```markdown
Export from Google:
□ Google Contacts → Export → vCard format
□ Save file to computer

Import to Proton:
□ Proton Mail → Settings → Import/Export
□ Import contacts → Choose file
□ Review and clean up duplicates
□ Organize into groups if needed
```

### Step 5: Calendar Migration

```markdown
Export from Google Calendar:
□ Google Calendar Settings → Import & Export
□ Export each calendar as .ics file

Import to Proton Calendar:
□ Proton Calendar → Settings → Import
□ Upload .ics file(s)
□ Assign to appropriate calendar
□ Set up calendar sharing if needed
```

### Step 6: The Update Sequence

```markdown
Priority Order for Email Updates:

Week 1 - Critical:
□ Banking/Financial
□ Government services
□ Healthcare
□ Insurance
□ Employer/School

Week 2 - Important:
□ Utilities
□ Phone carrier
□ Subscriptions
□ Shopping accounts
□ Password recovery emails

Week 3 - Social:
□ Social media
□ Forums/Communities
□ Newsletters
□ Friends/Family notification

Week 4 - Cleanup:
□ Remaining services
□ Update email signatures
□ Set up auto-responder on Gmail or just respond via Proton
```


### ProtonMail Organization

```markdown
Create Folders:
□ Archive
□ Financial
□ Receipts
□ Personal
□ Work
□ Newsletters

Set Up Filters:
□ Auto-sort newsletters
□ Flag important senders
□ Move receipts automatically
□ Filter promotional emails
```

---

## PART 4: BROWSER ECOSYSTEM

**Time Required: 2-3 hours**

### Step 1: Firefox Installation & Setup

```markdown
Download and Install:
□ Download from mozilla.org/firefox
□ Install with default settings
□ Sign in with Firefox account (temporary)
□ Import bookmarks from Chrome

Initial Configuration:
□ Settings → Privacy & Security
□ Enhanced Tracking Protection: Strict
□ Block cookies: All third-party
□ Send "Do Not Track": Always
□ HTTPS-Only Mode: Enable all windows
□ Ask to save passwords: OFF (using Bitwarden)
```

### Step 2: Essential Extensions

```markdown
Privacy Extensions:
□ Bitwarden - Password manager
□ uBlock Origin - Ad/tracker blocking

Below are optional, but remember that with each addition, you become more recognizable.

□ Obsidian snipping tool - snip web pages for offline reading
□ Privacy Badger - Additional tracking protection
□ HTTPS Everywhere - Force secure connections
□ ClearURLs - Remove tracking from URLs
□ Decentraleyes - Local CDN emulation

```

### Step 3: Search Engine Switch

```markdown
Change Default Search:
□ Settings → Search
□ Default Search Engine → DuckDuckGo
□ Remove other search engines except:
  - DuckDuckGo (primary)
  - Startpage (backup)
  - Searx (advanced)

For Occasional Google Needs:
□ Use "!g" prefix in DuckDuckGo
□ Opens Google search in container
□ Limits tracking capability
```

### Step 4: Bookmark Migration & Cleanup

```markdown
Import Process:
□ Chrome → Bookmarks → Export
□ Firefox → Bookmarks → Import from HTML
□ Organize into folders:
  - Daily Use
  - Work/Professional
  - Resources
  - Archive
□ Delete outdated bookmarks
□ Update login URLs to avoid Google OAuth
```

### Step 5: Chrome Data Cleanup

```markdown
Before Abandoning Chrome:
□ Export passwords (if not done)
□ Export bookmarks (if not done)
□ Save any important extensions list
□ Screenshot important settings
□ Download your Chrome data via Google Takeout

Final Chrome Cleanup:
□ Clear all browsing data
□ Sign out of Chrome
□ Uninstall (after 1-2 weeks successful Firefox use)
```

---

## PART 5: DOCUMENT/PHOTO LIBERATION

**Time Required: 3-4 hours (varies by data amount)**

### Step 1: Assess Your Google Storage

```markdown
Check Current Usage:
□ Google Drive storage amount
□ Google Photos count and size
□ Gmail storage used
□ Total across all services

Prioritize What to Migrate:
□ Active work documents
□ Important photos/memories
□ Financial/legal documents
□ Creative projects
□ Educational materials
```

### Step 2: Google Takeout Preparation

```markdown
Create Full Backup:
□ Go to takeout.google.com
□ Select data to export:
  - Drive
  - Photos
  - Gmail (if needed)
  - Contacts
  - Calendar
□ Choose file format:
  - .zip files
  - 2GB segments (easier to handle)
□ Delivery method: Email link
□ Create export (may take hours/days)
```

### Step 3: Set Up Proton Drive

```markdown
Initial Setup:
□ Access via web or install desktop app
□ Create folder structure:
  - Documents
  - Photos
  - Archives
  - Backups
  - Work
□ Enable auto-sync for desktop (optional)
```

### Step 4: Migration Strategy

```markdown
Phase 1 - Critical Documents:
□ Financial records
□ Legal documents
□ Medical records
□ Tax documents
□ Insurance papers

Phase 2 - Active Projects:
□ Current work files
□ Active creative projects
□ Educational materials
□ Reference documents

Phase 3 - Archives:
□ Old photos
□ Completed projects
□ Historical documents
□ Email archives
```

### Step 5: Photo Management

```markdown
Google Photos Alternative Strategy:

Option 1 - Proton Drive:
□ Create year/month folders
□ Upload in batches
□ Manual but private

Option 2 - Local + Cloud:
□ External drive for master copies
□ Proton Drive for favorites
□ NAS for home access (advanced)

Option 3 - Hybrid:
□ Keep Google Photos for sharing
□ Proton Drive for private photos
□ Local backup for everything
```

### Step 6: Document Alternatives

```markdown
Google Docs → LibreOffice:
□ Download LibreOffice (free)
□ Export Google Docs as .odt/.docx
□ Save to Proton Drive

Google Sheets → LibreOffice Calc:
□ Export sheets as .ods/.xlsx
□ Formulas may need adjustment
□ Test complex spreadsheets carefully

For Collaboration Needs:
□ CryptPad (encrypted)
□ Standard Notes (notes)
□ Joplin (notes with sync)
□ Or continue using Google for shared docs only
```

---

## PART 6: TEXT TO SIGNAL

**Time Required: 1 hour**

### Step 1: Install Signal

```markdown
Mobile Installation:
□ Download from App Store/Play Store
□ Verify it's by "Signal Foundation"
□ Open and allow permissions:
  - Contacts (to find friends)
  - Notifications (for messages)
  - Microphone/Camera (for calls)

Desktop Installation (after mobile):
□ Download from signal.org/download
□ Install desktop version
□ Link via QR code from mobile app
□ Settings → Linked Devices → Link New Device
```

### Step 2: Initial Configuration

```markdown
Basic Setup:
□ Create Signal PIN (remember it!)
□ Add to Bitwarden immediately
□ Set profile name (real or pseudonym)
□ Add profile photo (optional)
□ Enable Registration Lock:
  - Settings → Account → Registration Lock
  - Prevents SIM swap attacks

Privacy Settings:
□ Settings → Privacy:
  - Read Receipts: Your choice
  - Typing Indicators: Your choice
  - Screen Lock: Enable
  - Screen Security: Enable (blocks screenshots)
  - Incognito Keyboard: Enable
  - Always Relay Calls: Enable for IP hiding
```

### Step 3: Security Features

```markdown
Enable Key Features:
□ Disappearing Messages:
  - Per conversation setting
  - Default timer: 1 week suggested
  - Can set from 5 seconds to 4 weeks

□ View-Once Media:
  - Photos/videos delete after viewing
  - Tap the "1x" button when sending

□ Note to Self:
  - Encrypted notes to yourself
  - Syncs across devices
  - Good for passwords/sensitive info temporarily
```

### Step 4: Migration Strategy

```markdown
Week 1 - Close Circle:
□ Install Signal
□ Invite immediate family
□ Invite best friends
□ Move sensitive conversations

Week 2 - Extended Circle:
□ Invite friend groups
□ Work colleagues (if appropriate)
□ Create group chats
□ Share why you're switching

Week 3 - Full Migration:
□ Add Signal info to email signature
□ Update social media contact methods
□ Set SMS/WhatsApp to check less often
□ Inform remaining contacts
```

---

## PART 7: FULL MOVE

**Time Required: 2-3 hours**

### Testing Period Checklist

```markdown
Two-Week Verification:
□ All important emails arriving at Proton
□ Can access all moved documents
□ Password manager working smoothly
□ Browser meeting all needs
□ Signal conversations active
□ No critical services missed

Document What's Still on Google:
□ YouTube subscriptions
□ Google Maps saved places
□ Play Store purchases
□ Old email archives
□ Shared family services
□ Work/school requirements
```

### Google Account Decisions

```markdown
Option 1 - Complete Deletion (Highest Privacy):
□ Download all data via Takeout
□ Verify backups are complete
□ Check account deletion impact
□ Visit myaccount.google.com/delete-services-or-account
□ Follow deletion process
□ Note: PERMANENT - Cannot be undone

Option 2 - Dormant Maintenance (Recommended):
□ Remove payment methods
□ Delete unnecessary data
□ Set privacy settings to maximum
□ Remove phone number if possible
□ Use only for necessities (YouTube, etc.)
□ Check monthly for security

Option 3 - Minimal Active Use:
□ Keep for specific services
□ Use separate browser/profile
□ No personal data storage
□ Regular privacy checkups
□ Annual data deletion
```

### Account Recovery Verification

```markdown
Before Final Cutover:
□ Test password reset on critical accounts
□ Verify 2FA backup codes work
□ Confirm recovery emails updated
□ Document account recovery procedures
□ Test Bitwarden backup/restore
□ Have paper backup of critical info
```

### Final Cleanup Tasks

```markdown
Google Services:
□ Remove Google Pay cards
□ Export Google Maps lists
□ Download purchased apps list
□ Save YouTube subscriptions
□ Archive Google Voice texts
□ Export any Workspace data

Browser/Device:
□ Remove Google account from devices
□ Clear Chrome sync data
□ Uninstall unused Google apps
□ Remove Google DNS settings
□ Disable Google backup
□ Sign out of all Google services
```

### Celebrate!

```markdown
You've Successfully:
✓ Secured all passwords in Bitwarden
✓ Migrated email to ProtonMail
✓ Switched to privacy-focused browser
✓ Moved documents to Proton Drive
✓ Upgraded to Signal messaging
✓ Reduced Google's data collection by 90%+

Take a moment to appreciate this achievement!
```

---

## PART 8: LINUX & GRAPHENEOS

**Time Required: Research phase 2-3 hours, Implementation varies**

### Desktop: Linux Exploration

```markdown
Beginner-Friendly Distributions:
□ Linux Mint (most Windows-like)
□ Pop!_OS (great for newer hardware)
□ Ubuntu (most support/documentation)
□ Zorin OS (familiar interface)

Testing Without Commitment:
□ Try via USB boot (no installation)
□ Run in virtual machine first
□ Dual-boot with Windows initially
□ Full switch when comfortable

Key Benefits:
- No Microsoft telemetry
- Full control over updates
- Free and open source
- Better privacy by default
- Lighter on resources
```

### Mobile: GrapheneOS Overview

```markdown
What It Is:
□ Android without Google
□ Security-hardened OS
□ Privacy-focused design
□ Works on Pixel phones only

Consider If:
□ You own a compatible Pixel
□ Don't need banking apps (some don't work)
□ Can live without some apps
□ Want maximum mobile privacy
□ Comfortable with technical setup

Not Ready? iOS Alternative:
□ Use iPhone with:
  - iCloud disabled
  - Location services restricted
  - App permissions minimized
  - VPN always on
  - Little Snitch Firewall
```

---

## PART 9: ROUTER & NETWORKING (Optional Advanced)

**Time Required: Research 2 hours, Implementation 4-6 hours**

### Router Upgrade Path

```markdown
Entry Level:
□ Flash router with DD-WRT/OpenWrt
□ Basic firewall rules
□ DNS filtering
□ Guest network isolation

Intermediate:
□ Dedicated hardware (Protectli, etc.)
□ pfSense or OPNsense
□ VLAN segmentation
□ VPN at router level

Advanced:
□ Multiple network segments
□ IoT device isolation
□ Traffic monitoring
□ Self-hosted services
```

*Note: I have enjoyed the networking capabilites of the ASUS RT-AX86 Series with Asuswrt-Merlin firmware (https://www.asuswrt-merlin.net). I believe for most, this will be more than sufficient, though you will have to invest in a modem/ONT or use the ISP default with a firewall enabled on the device, or physical firewall in-between the router and modem/ONT.*
### Quick Network Hardening

```markdown
Can Do Today:
□ Change default router password
□ Update router firmware
□ Disable WPS
□ Use WPA3 (or WPA2 minimum)
□ Change network name (no personal info)
□ Disable router remote access
□ Enable guest network for visitors
□ Use Cloudflare (1.1.1.1) or Quad9 (9.9.9.9) DNS
``` 

---

## Support Resources

### Communities

- r/privacy (Reddit)
- Proton community forums
- Bitwarden community
- Signal community forum
- PrivacyTools.io
- Electronic Frontier Foundation

### Emergency Contacts

- Keep written record of:
    - Recovery emails
    - Backup phone numbers
    - Emergency contact person
    - Critical account numbers
    - Backup codes (paper)

---

## Final Notes

**This is a marathon, not a sprint.** Every person's journey will be different based on:

- Technical comfort
- Family/work requirements
- Threat model
- Available time
- Financial resources

**The goal isn't perfection** - it's sustainable improvement. Even completing just Parts 1-3 (Password Manager, Email, Browser) puts you ahead in terms of privacy.

**You're not alone** - Millions are making this transition. Use the communities, ask questions, and help others as you learn.

**Privacy is a practice** - not a product or destination. Keep learning, adjusting, and improving based on your needs.

Remember, you don't have to do everything. Do what makes sense for your life, your threat model, and your comfort level. Each step forward is a victory.