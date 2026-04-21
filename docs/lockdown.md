# Lockdown — securing the rented Macs

Running untrusted workloads on hardware you own is a minefield. Get this right on day one or it will bite you.

The goal is three layers:
1. **Protect you** from your customers' mistakes and bad actors
2. **Protect your customers** from each other (and from you — they should trust the platform)
3. **Protect your fleet's reputation** (IPs, hosting provider, legal)

---

## 1. Management layer — MDM

Don't try to secure Macs by hand. Enrol every device into an MDM before it ever touches a customer.

**Recommended (2026):**
- **Mosyle Business** — cheapest serious option (~£1–1.50 / device / month)
- **Kandji** — slicker UX, better automation, ~£3–4 / device / month
- **Jamf Now** — solid, more expensive, enterprise-leaning

What MDM gives you:
- Remote lock / wipe if a customer stops paying or abuses the machine
- Configuration profiles (firewall rules, login restrictions, no-bootcamp, etc.)
- Forced OS updates on a schedule
- Activation Lock management — absolutely critical if a Mac ever gets stolen from colo
- App install/block lists
- Compliance reporting

**Enrol via Apple Business Manager (ABM)** so devices are supervised and you have full control, not just the "polite" MDM powers you get from user enrolment.

---

## 2. OS hardening (apply via MDM profile)

### User accounts
- **One admin account for you.** Password in a password manager. Never shared.
- **One standard (non-admin) account for the customer.** They get it; you don't.
- Never give the customer sudo. If they need to install something that requires admin, you do it or you write a self-service tool.

### FileVault
- **Always on.** Enforce via MDM.
- Escrow the recovery key to your MDM, not to iCloud.

### Firewall
- Application firewall: on.
- Block all incoming except: SSH (22), VNC (5900) if used, and only from Tailscale IPs.
- Use the `pf` (Packet Filter) for finer-grained rules — block outbound SMTP (25, 587, 465) by default to kill spam potential.

### Gatekeeper & SIP
- Gatekeeper: on, "App Store and identified developers".
- System Integrity Protection (SIP): **never disable**.
- Code-signing requirement for launch agents / daemons: enforced.

### Network
- Disable unused interfaces (Bluetooth off unless required, Wi-Fi off if on Ethernet).
- No AirDrop, no Handoff, no Universal Clipboard (leak vectors).
- DNS: pin to a filtering resolver (NextDNS, Cloudflare 1.1.1.2 for malware blocking, or your own).

### Telemetry
- Ship logs to a central place (Datadog, Axiom, or even a self-hosted Loki).
- Watch for: failed SSH attempts, unusual outbound traffic, CPU sustained at 100% (possible mining).

---

## 3. Access control

### SSH
- **Key-only, no passwords.** Ever.
- Disable root login.
- `AllowUsers` whitelist — only the customer's account.
- Fail2ban or equivalent — even though you're on Tailscale, defence in depth.
- Bind sshd to the Tailscale interface IP only, not 0.0.0.0.

### Tailscale
- Every machine joins your tailnet with a tagged ACL.
- Customer gets their own tailnet user or a device key limited to their node.
- No public SSH endpoint. If a customer insists on public IP access, charge extra and log heavily.

### Remote desktop
- Screen Sharing (VNC) via Tailscale only.
- Or use **Jump Desktop Connect** (better UX, works over Tailscale).

---

## 4. Abuse prevention

This is where most indie hosts get burned. Bake these in from day one.

### Terms of Service — non-negotiable clauses
- No crypto mining (explicit list: XMR, RandomX, proof-of-work anything)
- No spam / bulk email sending
- No hosting phishing sites or malware
- No scraping that violates target sites' ToS at volume
- No CSAM (automatic termination + law enforcement referral, state explicitly)
- No reselling access

### Automated abuse detection
- **CPU watchdog:** sustained >90% CPU for 2+ hours = alert. Check workload.
- **Outbound bandwidth anomaly:** sudden spike = alert.
- **Outbound port 25/587/465:** block by default. Unblock only with written request and justification.
- **Known mining pool IPs:** block at the router.
- **DNS monitoring:** requests to known malicious domains trigger quarantine.

### IP reputation
- Use clean IP ranges from a hosting provider with a good reputation (not home broadband static IPs — they get blocklisted).
- Monitor your IPs with **IPVoid**, **AbuseIPDB**, **Spamhaus** weekly.
- Have a process to rotate IPs if one gets listed.

---

## 5. Physical security (once you're in colo)

- Locked cage or cabinet. Not an open shelf.
- Activation Lock enabled on all devices (via ABM).
- Serial numbers and MAC addresses logged in an inventory.
- Cameras at the colo (provider should have these).
- No customer data on the Mac that isn't encrypted — they bring their own keys for anything sensitive.

---

## 6. Incident response

Write a runbook before you need it. Minimum playbooks:

1. **"Customer's Mac is unresponsive"** → try remote restart via MDM → if fails, colo hands-on within 4 hours.
2. **"Abuse report from upstream"** → isolate Mac via MDM, open ticket with customer, 24-hour response window.
3. **"Suspected compromise"** → pull from network, image for forensics, wipe and reprovision from clean baseline.
4. **"Hardware failure"** → swap to spare, restore customer's snapshot, ship failed unit back to Apple.
5. **"Customer hasn't paid in 14 days"** → lock via MDM, email sequence, wipe after 30 days per ToS.

---

## 7. Backups & snapshots

- Nightly snapshot of customer's home directory (not system) to an encrypted off-device store (Backblaze B2 is cheap).
- Retain for 14 days rolling.
- Customer can request restore via a support ticket. Document the RPO/RTO in your SLA.
- Don't back up full disk images of customer machines — legal and storage nightmare.

---

## 8. Legal (UK-specific)

Talk to an actual solicitor before launch, but at minimum:

- **Privacy policy** — what data you collect, for how long, UK GDPR compliant
- **ToS** — the abuse policy above, plus payment terms, termination conditions, liability caps
- **Business structure** — Limited company, not sole trader. Insurance for professional indemnity.
- **Data processing agreement (DPA)** — offer one; enterprise buyers will ask
- **ICO registration** — you'll process personal data, so register as a data controller (~£40/year)
- **Export control** — be aware that macOS and some software have US export restrictions; don't sell to sanctioned jurisdictions

---

## 9. Quick-start checklist

Before the first customer gets access:

- [ ] Apple Business Manager set up
- [ ] MDM chosen and configured
- [ ] Baseline configuration profile tested on one Mac
- [ ] Firewall rules tested (can you break in? your customer shouldn't)
- [ ] Tailscale ACLs written and tested
- [ ] Abuse detection pipeline running
- [ ] Clean IP ranges confirmed
- [ ] ToS written and linked from the landing page
- [ ] Incident runbooks written
- [ ] Off-site snapshot backups running
- [ ] Inventory spreadsheet (serial, MAC, IP, tenant, start date)
- [ ] Business insurance active
- [ ] ICO registration done
