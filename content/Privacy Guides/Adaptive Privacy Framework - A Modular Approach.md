## Core Philosophy

This document is meant to serve as a starting point for your privacy journey. The steps below are general and mostly vague with the hopes of introducing you to new ways of approaching your digital life. This isn't meant to take anything away, but provide private or anonymous ways of interfacing with our complex digital environments. Generally everything we do has our data attached to it, and my hope is that we can minimize our exposure, and secure our digital lives. I aim to take back a sense of quietude and ownership over our spaces and devices so we can live in peace without the anxiety of constantly being observed.

1. **Zero Data Loss** - Nobody loses access to anything important
2. **Gradual Migration** - Run parallel systems before cutting over
3. **Reversibility** - Can backtrack if something goes wrong
4. **One Weekend Per Project** - Each major section completed in a weekend
## 1. Initial Assessment Module

### 1.1 Threat Model Definition

**Choose Your Privacy Level Intent:**

- **Level 1 - Basic Privacy**: Reduce spam, limit ad tracking, basic identity protection
	- *This can be as simple as adding uBlock Origin to a FireFox web browser and begin segmenting emails.*
- **Level 2 - Enhanced Privacy**: Protection from data brokers, social engineering, minor harassment
	- *This will require intentional removal of data consistently, and a long term plan for reclamation of data from Google, Meta, etc.*
- **Level 3 - High Privacy**: Professional protection, activist/journalist needs, stalking
	- *I would recommend hiring a professional and/or reading Michael Bazzel's 'Extreme Privacy: What it takes to Disappear - 5th Edition' if your concerns go beyond level two here.*

Michael Bazzel's website:
https://inteltechniques.com/book7.html

### 1.2 Digital Footprint Audit

Take an afternoon to do an OSINT investigation on yourself. Open a private browsing window, and begin your hunt for any information you can find.

- [ ] Google yourself. This includes name variations, old usernames, etc.
	- [ ] *You can use Google dorking here to get as granular as you would like
	- [ ] https://dorksearch.com/blog/beginners-guide-google-dorking/*
- [ ] Check HaveIBeenPwned for password/email breaches
- [ ] List all email addresses (active and abandoned)
- [ ] Document all online accounts and compile all information to have an overview of your complete footprint.


---

## 2. Foundation

### 2.1 Email Hygiene

**Actions:**

1. Create compartmentalized email structure:
    - Personal (friends/family only)
    - Professional (work/career)
    - Commercial (shopping/services)
    - Throwaway (trials/untrusted sites)

**Tools:**

- ProtonMail for sensitive communications
- SimpleLogin for email aliasing
- Temporary email services such as 'TenMinuteMail' for one-time uses

You can begin forwarding your Gmail into your ProtonMail inbox with this walkthrough - 

https://proton.me/support/automatic-forwarding-gmail



### 2.2 Password Infrastructure

1. [ ] Install/download password manager (Bitwarden/1Password)
2. [ ] Generate unique 16+ character passwords
3. [ ] Enable 2FA on all critical accounts
4. [ ] Use hardware keys for high-value accounts (optional)
5. [ ] Create password recovery documentation/location
6. [ ] Encrypt sensitive keys with encryption software like Kleopatra (optional)

### 2.3 Browser Configuration

**Privacy-First Setup:**

- Primary: Firefox with uBlock Origin, Privacy Badger(optional)
- Secondary: Brave for compartmentalized browsing
	- *Chromium browsers are powerful software, so the trust level between you and the software provider should always be in question when using Chrome or a Chrome variant like Brave*
- Tertiary: Tor Browser for sensitive/anonymous research



---

## 3. Data Minimization Module

### 3.1 Social Media Audit

**Reduction Strategy:**

1. Download all data from platforms
2. Delete unnecessary posts/photos 
3. Adjust privacy settings to maximum
4. Remove real name where possible
5. Unlink phone numbers

### 3.2 Data Broker Opt-Out

**Priority Targets:**

```markdown
Tier 1 (Immediate):
- Whitepages
- BeenVerified
- Spokeo
- Intelius
- TruthFinder

Tier 2 (Within 30 days):
- MyLife
- RadarOnline
- FastPeopleSearch
- TruePeopleSearch
- FamilyTreeNow
```

### 3.3 Marketing Opt-Out

- DMAchoice.org registration
- OptOutPrescreen for credit offers
- National Do Not Call Registry
- CatalogChoice for physical mail

---

## 4. Identity Compartmentalization Module

### 4.1 Digital Identity Separation

**Create Distinct Personas:**

```markdown
Professional Identity:
- LinkedIn/professional networks only
- Business email
- Professional photo
- Real name (if required)

Personal Identity:
- Private social media
- Pseudonym consideration
- Separate email/phone
- Limited photo sharing

Anonymous Identity:
- Forums/communities
- No real information
- VPN/Tor access
- Cryptocurrency payments
```

### 4.2 Financial Privacy

**Implementation Path:**

1. Privacy.com virtual cards for online purchases
2. Separate checking for online transactions
3. Cryptocurrency for anonymous purchases
4. Cash for local sensitive purchases

### 4.3 Communication Channels

```markdown
Tier Structure:
- Public: Standard email/SMS
- Private: Signal/WhatsApp
- Sensitive: Signal with disappearing messages
- Anonymous: Session/Briar
```

---

## 5. Operational Security Module

### 5.1 Device Hardening

**Progressive Implementation:**

Phase 1 (Basic):

- [ ] Full disk encryption
- [ ] Automatic screen locks
- [ ] Regular OS updates
- [ ] Antivirus/antimalware

Phase 2 (Enhanced):

- [ ] Separate user accounts
- [ ] Virtual machines for risky activities
- [ ] Network segmentation
- [ ] DNS over HTTPS

Phase 3 (Advanced):

- [ ] Linux primary OS
- [ ] TailsOS on separate device for compartmentalization
	- [ ] *I reccomend not using a previously used device, or anything that can be traced back to you, or previous accounts or locations*
- [ ] Router with opensource firmware/Hardware firewall
- [ ] BIOS/UEFI passwords
- [ ] Encrypted USB, hard drives, etc.

### 5.2 Location Privacy

**Graduated Approach:**

- Level 1: Disable location for apps that don't need it (GrapheneOS has an amazing config for this)
- Level 2: Use VPN consistently, disable GPS when not needed
- Level 3: Faraday bags, location spoofing, cash-only travel
- Level 4: Full bug-out protocols (burner devices, no personal electronics)

### 5.3 Network Security

```markdown
Implementation Tiers:
Basic:
- WPA3 on home WiFi
- VPN for public WiFi
- HTTPS everywhere

Intermediate:
- pfSense/OPNsense firewall
- VLAN segmentation
- Pi-hole for ad/tracking blocking

Advanced:
- Dedicated privacy router
- Tor integration
- Multiple VPN chains
```

---

## 6. Maintenance Module (All Levels)

### 6.1 Regular Audits

**Quarterly Tasks:**

- [ ] Google yourself with new search terms
- [ ] Check data broker sites for reappearance
- [ ] Review account permissions/connected apps
- [ ] Update passwords for critical accounts
- [ ] Test backup access methods
- [ ] Review privacy settings on all platforms

### 6.2 Incident Response Plan

**If Compromise Detected:**

1. Document the breach
2. Change all passwords starting with financial
3. Enable 2FA everywhere possible
4. Contact financial institutions
5. File reports if necessary

### 6.3 Documentation System

**Maintain Records Of:**

- Account creation in password manager
- Privacy setting changes
- Opt-out confirmations
- Security incidents
- Contact information for support

---

## 7. Implementation Workflow

### Phase 1: Foundation (Week 1-2)

```markdown
Day 1-3: Assessment and threat modeling
Day 4-7: Password manager and 2FA setup
Day 8-10: Email restructuring
Day 11-14: Browser configuration and basic hardening
```

### Phase 2: Reduction (Week 3-6)

```markdown
Week 3: Social media audit and cleanup
Week 4: Data broker opt-outs (Tier 1)
Week 5: Marketing opt-outs and data broker (Tier 2)
Week 6: Account closure for unused services
```

### Phase 3: Enhancement (Week 7-12)

```markdown
Week 7-8: Identity compartmentalization
Week 9-10: Financial privacy implementation
Week 11-12: Communication channel migration
```

### Phase 4: Maintenance (Ongoing)

```markdown
Monthly: Quick security check
Quarterly: Full audit
Annually: Complete threat model review
```

---

## 8. Decision Trees

### "Should I Delete This Account?"

```
Is it actively used? 
├─ No → Can it be deleted?
│      ├─ Yes → Delete and document
│      └─ No → Minimize data, max privacy settings
└─ Yes → Is it necessary?
       ├─ No → Plan migration, then delete
       └─ Yes → Minimize exposure, compartmentalize
```

---

## 9. Tool Recommendations by Threat Level

### Level 1 (Basic)

- **Password Manager**: Bitwarden (free)
- **Browser**: Firefox + uBlock Origin
- **Email**: ProtonMail with 2FA
- **VPN**: Proton or IVPN

### Level 2 (Enhanced)

- **Browser**: Hardened Firefox + Brave
- **Email**: ProtonMail/Tutanota
- **VPN**: Multiple providers
- **Phone**: GrapheneOS or iOS with restrictions

### Level 3 (High)

- **Password Manager**: Self-hosted Bitwarden
- **Browser**: Tor + hardened browsers
- **Email**: Self-hosted or ProtonMail with custom domain
- **VPN**: Multiple + Tor
- **Phone**: GrapheneOS with minimal apps
- **OS**: Linux (PopOS, Ubuntu)

---

## 10. Common Pitfalls to Avoid

1. **Over-compartmentalization**: Don't create so many identities you can't maintain them
2. **Perfect as enemy of good**: Start with basics, iterate
3. **Social isolation**: Balance privacy with maintaining relationships
4. **Forgetting physical security**: Digital privacy needs physical security
5. **Inconsistent practices**: One leak can compromise everything

---

## Notes

This framework is designed to be **modular**:

- Start at any level appropriate to their threat model
- Mix and match modules based on specific needs
- Progress gradually without overwhelming changes
- Practical privacy that doesn't disrupt daily life
- Graduated responses to different threat levels
- Clear decision points for escalation
- Maintainable long-term practices

## TLDR:

1. **Start with getting your online identities in check**
    
    - Gmail to Proton
    - Throwaway accounts to SimpleLogin (you can sync this with proton)
    - Use ProtonMail alternate identities to create alias for specific accounts
    - Passwords and 2FA all logged and accounted for with a simple search of your password database
2. **Create a simple and effective ecosystem**
    
    - Uninstall unnecessary apps from mobile devices
    - Encrypted communication from texts to emails
    - Own your own router
    - Switch to Linux if at all possible
3. **Take time to assess as you migrate between environments**
    
    - Make sure you have everything you need before you shut down accounts
    - Make sure your 2FA and passwords are accounted for
    - Request and delete data from sites or companies as you move away from them to scrub your information from data brokers

*Note: You can refer to the Privacy Implementation Guide for somewhat more detailed instructions.*

---

**Privacy is a journey, not a destination.**

Start where you are, improve gradually, and adjust based on your evolving needs.