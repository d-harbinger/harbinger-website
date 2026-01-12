# Browser Hardening Guide 2025-2026

**Last Updated**: January 5, 2026  
**Sources**: Verified from arkenfox user.js, ArchWiki, PrivacyTests.org, Brave documentation  
**Confidence Level**: HIGH - Based on current best practices and vendor documentation

---

## Table of Contents

1. [[#Firefox Hardening]]
2. [[#Brave Hardening]]
3. [[#Why Distrust Chrome]]

---


> [!info] **Further Reading**
> 
> - Arkenfox user.js Wiki: https://github.com/arkenfox/user.js/wiki
> - PrivacyTools.io: https://www.privacytools.io
> - PrivacyTests.org: https://privacytests.org (browser comparison)
> - EFF Cover Your Tracks: https://coveryourtracks.eff.org
> - Brave Privacy Updates: https://brave.com/privacy-updates/


---

## Firefox Hardening

> [!warning] **CRITICAL - Backup First** Before making ANY changes:
> 
> Consider backing up bookmarks and keeping them in a location you can reference later if needed, your profile if you are interested (though I recommend not being signed in to any account that can be tracked)
> 
> 1. Navigate to `about:support` → "Profile Folder" → "Open Folder"
> 2.  Close Firefox completely
> 3. Copy entire profile folder to safe location
> -or-
>  Consider not logging in at all

### Initial Settings

#### 1. Disable Telemetry & Data Collection

**Path**: `Menu (≡) → Settings → Privacy & Security → Firefox Data Collection and Use`

> [!check] **Actions Required**
> 
> - [ ] Uncheck "Allow Firefox to send technical and interaction data to Mozilla"
> - [ ] Uncheck "Allow Firefox to install and run studies"
> - [ ] Uncheck "Allow Firefox to send backlogged crash reports"

**Source**: Mozilla collects telemetry by default - this is first-line defense.

---

#### 2. Enhanced Tracking Protection

**Path**: `Settings → Privacy & Security → Enhanced Tracking Protection`

> [!check] **Recommended Configuration**
> 
> - [ ] Select **"Strict"** (blocks most trackers, may break some sites)
> - [ ] Enable **"Send websites a 'Do Not Track' signal"** → Set to **"Always"**

**Note**: DNT is deprecated in favor of Global Privacy Control (GPC), but Firefox supports both.

---

#### 3. Cookies & Site Data

**Path**: `Settings → Privacy & Security → Cookies and Site Data`

> [!check] **Configuration**
> 
> - [ ] Enable "Delete cookies and site data when Firefox is closed" (optional but recommended)
> - [ ] **DO NOT** check "Block cookies and site data" globally - use per-site controls instead

---

#### 4. HTTPS-Only Mode

**Path**: `Settings → Privacy & Security → HTTPS-Only Mode`

> [!check] **Enable**
> 
> - [ ] Select **"Enable HTTPS-Only Mode in all windows"**

**Rationale**: Eliminates unencrypted HTTP connections, mitigates MITM attacks.

---

#### 5. DNS over HTTPS (DoH)

**Path**: `Settings → Privacy & Security → DNS over HTTPS`

> [!warning] **Considerations** DoH encrypts DNS queries but routes them through specific providers (Cloudflare, NextDNS by default).
> 
> **Options**:
> 
> - **Enable** if you trust Cloudflare/NextDNS more than your ISP
> - **Disable** if you run your own DNS resolver (pi-hole, unbound, Quad9 etc.)
> 	- Or if you followed the [[AsusWRT-Merlin Hardened Router Install Guide]] you should have your own DNS resolution in place.

To disable completely:

- [ ] Set to "Off" in GUI, OR
- [ ] Set `network.trr.mode` to `5` in `about:config` (disabled by choice, won't be overridden)

**Source**: ArchWiki confirms `5` prevents Mozilla from re-enabling DoH.

---

#### 6. Search Engine

**Path**: `Settings → Search`

> [!check] **Recommendation** Replace default search engine with privacy-respecting alternative:
> 
> - [ ] **DuckDuckGo** (zero tracking, Bing API backed)
> - [ ] **Brave Search** (independent index, no tracking)
> - [ ] **Startpage** (anonymized Google results)

Rationale: Google Search = comprehensive tracking + ad profiling.

---

#### 7. Disable Autofill

**Path**: `Settings → Privacy & Security → Logins and Passwords` + `Forms and Autofill`

> [!check] **Actions**
> 
> - [ ] Uncheck "Ask to save passwords" (use dedicated password manager instead)
> - [ ] Uncheck "Autofill addresses"
> - [ ] Uncheck "Autofill credit cards"

**Rationale**: Browser autofill = data exposure risk if compromised. Use KeePassXC, Bitwarden, etc.

---

### Advanced Hardening (`about:config`)

> [!warning] **PROCEED WITH CAUTION** These settings modify Firefox's internal configuration. Incorrect values can break functionality.
> 
> Access: Type `about:config` in address bar → Accept Risk

---

#### WebRTC Leak Prevention

**Issue**: WebRTC exposes your real IP address even when using VPN.

> [!check] **Fix**
> 
> ```
> media.peerconnection.enabled = false
> ```

**Impact**: Disables WebRTC entirely. May break video conferencing (Zoom, Meet, Jitsi), P2P file sharing.

**Alternative** (less restrictive):

```
media.peerconnection.ice.default_address_only = true
media.peerconnection.ice.no_host = true
```

---

#### Disable Prefetching & Speculative Connections

**Issue**: Firefox preloads links/resources before you click, leaking browsing intent.

> [!check] **Disable All Prefetch**
> 
> ```
> network.prefetch-next = false
> network.dns.disablePrefetch = true
> network.predictor.enabled = false
> network.http.speculative-parallel-limit = 0
> ```

---

#### Referrer Header Control

**Issue**: Referrer headers tell websites where you came from, enabling cross-site tracking.

> [!check] **Strict Referrer Policy**
> 
> ```
> network.http.referer.XOriginPolicy = 2
> network.http.referer.XOriginTrimmingPolicy = 2
> ```

**Values**:

- `XOriginPolicy = 2` → Only send referrer to same domain
- `XOriginTrimmingPolicy = 2` → Only send origin (not full URL)

---

#### Resist Fingerprinting

**Issue**: Websites fingerprint browser via canvas, WebGL, fonts, screen size, etc.

> [!check] **Enable RFP (Resist Fingerprinting)**
> 
> ```
> privacy.resistFingerprinting = true
> ```

**Side Effects**:

- Timezone → UTC
- Screen resolution → rounded to common values (1920x1080, etc.)
- Some websites may detect "bot-like" behavior

**Source**: Feature ported from Tor Browser, confirmed active in Firefox 135+.

---

#### Disable Pocket Integration

```
extensions.pocket.enabled = false
```

---

#### Disable Firefox Screenshots (Very optional)

```
extensions.screenshots.disabled = true
```

---

#### Disable Safe Browsing (Optional - Reduces Google Contact)

> [!warning] **SECURITY vs PRIVACY TRADEOFF** Safe Browsing checks URLs against Google's malware/phishing database.

```
browser.safebrowsing.malware.enabled = false
browser.safebrowsing.phishing.enabled = false
browser.safebrowsing.downloads.enabled = false
```

**Only disable if**:

1. You use alternative security (DNS filtering, uBlock Origin lists)
2. **You understand the risk**

Here you can use both Google's malware/phishing database, and your own uBlock and DNS filtering. I would consider telemetry a lesser evil to truly malicious websites since we are hardening our identities and segmenting things in other ways to prevent try identity leaks.

---

### Essential Extensions

> [!check] **Minimal Extension Stack**
> 
> 1. **uBlock Origin** (block ads, trackers, malware domains)
>     - Enable all filter lists in settings
>     - Add "EasyList Cookie" list
> 2. **Cookie AutoDelete** (auto-clear cookies on tab close)
> 3. **CanvasBlocker** (defeats canvas fingerprinting)
> 4. (Optional) **Multi-Account Containers** (isolate browsing contexts)

**Warning**: More extensions = more unique fingerprint. Balance carefully.

---

### user.js Implementation

For advanced users, use the **arkenfox user.js** template:

```bash
# Download arkenfox user.js
cd ~/path/to/firefox/profile
wget https://raw.githubusercontent.com/arkenfox/user.js/master/user.js

# Review file (CRITICAL - understand every setting)
less user.js

# Restart Firefox
```

**Source**: Arkenfox is community-maintained, audited, updated regularly. Gold standard for Firefox hardening, but as always before curling or wgetting, double check your git and your code.

---

### Verification

Test your browser fingerprint:

> [!check] **Testing Sites**
> - https://www.dnsleaktest.com/ (IVPN DNS check)
> - https://coveryourtracks.eff.org (EFF's fingerprinting test)
> - https://browserleaks.com (comprehensive leak tests)
> - https://amiunique.org (fingerprinting uniqueness)
> - https://www.deviceinfo.me (check WebRTC leaks)

---

## Brave Hardening

> [!info] **Key Difference from Firefox** Brave is Chromium-based but heavily modified for privacy. Most protections are **enabled by default** - hardening focuses on fine tuning.

### Initial Assessment

**Out-of-box Privacy (Brave 1.73+)**:

- ✅ Ad blocking (aggressive by default)
- ✅ Tracker blocking (fingerprinting protection: standard)
- ✅ HTTPS Everywhere (automatic upgrades)
- ✅ Script blocking (shields)
- ✅ Cookie partitioning (ephemeral 3rd-party storage)
- ✅ WebRTC leak protection (non-proxied UDP disabled)

**Source**: Brave Shields set to "Aggressive" by default since v1.73.

---

### Settings Review

#### 1. Shields Configuration

**Path**: `brave://settings/shields`

> [!check] **Optimal Settings (Can break sites, so test your favorite sites to make sure you can still access everything)**
> 
> - [ ] **Trackers & ads blocking** → **Aggressive**
> - [ ] **Upgrade connections to HTTPS** → **Enabled**
> - [ ] **Block scripts** → **Disabled** (enable per-site if needed)
> - [ ] **Block Cookies** → **Block third-party cookies**
> - [ ] **Block Fingerprinting** → **Strict** (may break some sites)

**Note**: "Strict" fingerprinting = randomized values. "Standard" = generic values.

---

#### 2. Privacy & Security

**Path**: `brave://settings/privacy`

> [!check] **Key Toggles**
> 
> - [ ] **WebRTC IP Handling Policy** → Set to **"Disable non-proxied UDP"**
>     - Prevents WebRTC IP leaks while preserving video call functionality
> - [ ] **Send a "Do Not Track" request** → **Enabled**
> - [ ] **Send P3P header** → **Disabled** (deprecated)
> - [ ] **Safe Browsing** → **Standard Protection** (Brave-proxied, not direct to Google)
> - [ ] **Allow Google Sign-In** → **Disabled**
> - [ ] **Allow Facebook logins** → **Disabled**
> - [ ] **Allow Twitter embedded tweets** → **Disabled**
> - [ ] **Allow LinkedIn embedded posts** → **Disabled**

**Rationale**: Disabling social logins prevents cross-site identity correlation.

---

#### 3. Search Engine

**Path**: `brave://settings/search`

> [!check] **Recommendation**
> 
> - [ ] Set default to **Brave Search** (independent index, no tracking)

Alternative: DuckDuckGo, Startpage, Searx

---

#### 4. Autofill

**Path**: `brave://settings/autofill`

> [!check] **Disable All**
> 
> - [ ] Passwords → Off (use external manager)
> - [ ] Payment methods → Off
> - [ ] Addresses → Off

---

#### 5. Brave Wallet (Crypto)

**Path**: `brave://settings/wallet`

> [!warning] **Privacy Consideration** Brave Wallet connects to blockchain RPC nodes (Infura, Alchemy by default).

**Options**:

1. **Disable entirely** if not using crypto
2. **Use with self-hosted node** (e.g., your own Ethereum node)

```
brave://settings/wallet
→ Default Ethereum wallet → None
```

---

#### 6. Brave Rewards (BAT Ads)

**Path**: `brave://settings/rewards`

> [!warning] **Data Implications** If enabled, Brave Rewards:
> 
> - Analyzes browsing locally (on-device)
> - Sends anonymized ad confirmation data (cannot be linked to individual)
> - Uses "STAR" protocol (k-anonymity guarantee)

**Privacy-conscious choice**: **Disable entirely**

```
Brave Rewards → Off
```

**Source**: Brave's privacy blog confirms local processing + k-anonymity for ad matching.

---

#### 7. Tor Windows

Brave includes built-in Tor routing for private windows.

> [!check] **Usage**
> 
> ```
> File → New Private Window with Tor
> ```

> [!warning] **LIMITATIONS**
> 
> - NOT full Tor Browser (lacks some anti-fingerprinting hardening)
> - Use for IP anonymity, NOT high-threat scenarios
> - DO NOT log into personal accounts in Tor windows

**Source**: Brave docs acknowledge this is "Tor lite", not Tor Browser replacement.

---

### Advanced Brave Flags

**Path**: `brave://flags`

> [!check] **Privacy-Enhancing Flags**
> 
> ```
> #brave-debounce → Enabled
> (Removes tracking parameters from URLs)
> 
> #brave-ephemeral-storage → Enabled
> (3rd-party storage cleared on site close)
> 
> #brave-block-screen-fingerprinting → Enabled
> (Randomizes screen size data)
> ```

**Note**: Flags may change between versions. Verify in current release.

---

### Differences: Brave vs Firefox

|Feature|Firefox|Brave|
|---|---|---|
|**Substrate**|Gecko (independent)|Chromium (Google-derived)|
|**Default Privacy**|Moderate (needs hardening)|Strong (aggressive by default)|
|**Fingerprinting**|Resist Fingerprinting (uniformity)|Randomization + blocking|
|**Ad Blocking**|Requires uBlock Origin|Built-in (aggressive)|
|**WebRTC**|Leaks IP (must disable)|Non-proxied UDP disabled by default|
|**Tor Integration**|None (use Tor Browser)|Built-in Tor windows|
|**Google Ties**|None (Mozilla)|Chromium base (but proxied/stripped)|
|**Customization**|Extensive (about:config)|Limited (flags + GUI)|
|**Extension Support**|Firefox addons|Chrome extensions|

---

### Brave Caveats

> [!warning] **Chromium Concerns in Brave**
> 
> 1. **Hangout Services Extension**: Disabled by default (v1.73+), will be removed
>     - Previously sent hardware metrics to Google
> 2. **Chromium Updates**: Brave tracks Chromium releases but strips Google APIs
> 3. **Sync**: Brave Sync is client-encrypted (unlike Chrome), does NOT touch Google servers

**Source**: July 2024 discovery confirmed "hangout_services" disabled in Brave; February 2025 update confirms complete removal.

---

### Verification

Test Brave's privacy posture:

> [!check] **Testing**
> 
> - https://privacytests.org (compare browsers)
> - Check Shields icon on any site (see blocked elements)
> - https://browserleaks.com/webrtc (verify no IP leak)

---

## Why Distrust Chrome

> [!warning] **CRITICAL CONTEXT** Chrome is the **least privacy-respecting mainstream browser** due to Google's advertising business model. Key issues:

---

### 1. Third-Party Cookie Retention

**Event**: Google reversed its 2020 promise to phase out 3rd-party cookies.

- **July 2024**: Announced cookie deprecation cancellation
- **October 2025**: Officially killed Privacy Sandbox (cookie replacement project)
- **April 2025**: Confirmed cookies remain indefinitely

**Impact**: Cross-site tracking remains the default. Chrome users are persistently profiled.

**Source**: Google's Anthony Chavez confirmed Privacy Sandbox retirement October 21, 2025.

---

### 2. Extensive Telemetry & Data Collection

Chrome collects:

- **Search history** (even in "signed-out" mode)
- **Browsing history** (synced to Google account)
- **Location data** (even when "Location History" disabled - confirmed by AP investigation 2018)
- **Form autofill data**
- **Click/mouse movement patterns** (per 2024 leaked internal documents)

> [!warning] **Incognito Mode is NOT Private**
> 
> - Websites still track you
> - ISP sees traffic
> - Google account activity still logged if signed in
> - Extensions may still record data

**Source**: AP investigation (2018), Google vs. DOJ antitrust testimony (2024).

---

### 3. Chromium "Hangout Services" Backdoor

**Discovery** (July 2024): All Chromium browsers ship with `hangout_services` extension.

**Data Sent to Google**:

- CPU/GPU usage
- Memory usage
- Processor details (model, cores, architecture)
- User visits to Google domains (Meet, Hangouts)

**Purpose**: Ostensibly to "optimize Google Meet performance" - in reality, gives Google services unfair advantage.

**Affected Browsers**: Chrome, Edge, Opera, Vivaldi, (old Brave builds)  
**Not Affected**: Firefox, LibreWolf, Tor Browser

**Source**: July 19, 2024 disclosure on AlternativeTo; confirmed via Chromium source code analysis.

---

### 4. API Privilege Asymmetry

Google reserves **exclusive APIs** in Chromium for its own services:

- Performance optimization hooks (Meet, Duo)
- Enhanced media capabilities
- Preferential caching

**Result**: Google services perform better in Chrome than competitors' services, creating anti-competitive moat.

**Regulatory Status**: Under investigation by EU (Digital Markets Act violation potential).

---

### 5. Fingerprinting Expansion (2025)

**Google's New Strategy** (announced February 2025):

- Shift from cookies → **digital fingerprinting**
- Tracks across devices: smartphones, smart TVs, gaming consoles, IoT
- Combines hardware config + behavioral patterns → persistent user ID

> [!warning] **Cannot Be Cleared Like Cookies** Fingerprinting persists across:
> 
> - Browser resets
> - Incognito mode
> - VPN usage (unless combined with anti-fingerprinting tools)

**Source**: UK ICO warnings (2025), Google's "Privacy Enhancing Technologies" rollout announcement.

---

### 6. Gemini AI Integration

**Q4 2025**: Chrome integrates Gemini AI assistant.

**Privacy Concerns**:

- Browsing context sent to Google's AI servers for processing
- Potential training data for future models
- No clear opt-out without disabling AI features entirely

**Source**: Competitor warnings from Apple (iPhone users advised to avoid Chrome), Microsoft (Windows warnings).

---

### 7. Market Dominance = Privacy Harm Amplification

Chrome commands **72% global browser market share** (2025).

**Implication**: Google's privacy-invasive defaults affect 3+ billion users, normalizing surveillance capitalism.

**Comparison**:

- Firefox: ~3% share, zero tracking by default
- Brave: ~1% share, aggressive anti-tracking by default

---

### Chromium vs Chrome: Key Distinctions

|Aspect|Chromium (Open Source)|Google Chrome (Proprietary)|
|---|---|---|
|**Telemetry**|Minimal (depends on build)|Extensive (Google Analytics)|
|**API Calls**|Some Google APIs present|Full Google integration|
|**Auto-Updates**|None (manual)|Google-controlled|
|**Sync**|Not available|Google account sync (unencrypted)|
|**Media Codecs**|Limited (licensing)|Full (proprietary)|

---

### Why Chromium-Based ≠ Chrome

**Clarification**: Browsers like Brave, Edge, Vivaldi use the Chromium _rendering engine_ but:

1. Strip Google-specific services
2. Add privacy layers
3. Replace sync mechanisms
4. Proxy/disable telemetry

**Critical Nuance**: Brave is Chromium-based but NOT Chrome. It removes Google's surveillance infrastructure.

**Source**: Brave's "ungoogled-chromium" approach documented in privacy blog.

---

### Regulatory & Ethical Context

**Ongoing Investigations**:

- **US DOJ Antitrust Case** (2024-2025): Chrome's preferential API access
- **EU DMA Violations**: Potential €10B+ fines for self-preferencing
- **UK ICO**: "Level playing field" demands by 2025

**Google's Response**: Claims Privacy Enhancing Technologies (PETs) balance ads + privacy. Critics argue it's "privacy theater."

---

### Bottom Line: Chrome = Maximum Convenience, Minimum Privacy

> [!warning] **Chrome Use Cases Where Privacy Is Sacrificed**
> 
> - Google Workspace integration (Docs, Meet, Calendar)
> - Chromecast/Android ecosystem
> - Extensions requiring Chrome Web Store
> - Enterprise MDM (managed browsers)

**Alternative Recommendation**:

- **Privacy-focused**: Firefox (hardened) or LibreWolf
- **Chromium-based privacy**: Brave or ungoogled-chromium
- **Maximum anonymity**: Tor Browser

---

## Threat Model Considerations

### Low Threat (Casual Privacy)

- **Firefox** (basic hardening + uBlock Origin)
- **Brave** (defaults acceptable)

### Medium Threat (Avoid Corporate Tracking)

- **Firefox** (full about:config hardening + arkenfox user.js)
- **Brave** (strict shields + disabled Rewards/Wallet)
- VPN recommended

### High Threat (Journalist, Activist, Nation-State Concern)

- **Tor Browser** (ONLY option - do not substitute)
- **Qubes OS** (compartmentalized VMs)
- **Tails** (amnesic live OS)
- Firefox/Brave inadequate for this threat level

---

## Changelog

**2025-01-05**: Initial release based on:

- Firefox 135+ (Q1 2025 feature set)
- Brave 1.73+ (Q4 2024/Q1 2025 builds)
- Chrome Privacy Sandbox sunset (October 2025)
- Chromium hangout_services disclosure (July 2024)

**Maintenance**: This guide should be updated quarterly as browser releases introduce new privacy features/concerns.

---