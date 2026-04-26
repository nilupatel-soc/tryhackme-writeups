# Pyramid of Pain — TryHackMe Writeup

**Analyst:** Nirali Patel  
**Date:** April 2026  
**Platform:** TryHackMe — SOC Level 1 Path  
**Difficulty:** Easy  
**Category:** Threat Intelligence / SOC Fundamentals  
**Room Link:** https://tryhackme.com/room/pyramidofpainax  

---

## 📌 Overview

The Pyramid of Pain is a threat intelligence model created 
by security researcher David Bianco. It describes how 
difficult it is for an attacker to change different types 
of indicators when defenders detect and block them.

The higher up the pyramid, the more pain it causes the 
attacker — and the more valuable it is for defenders to 
detect those indicators.

---

## 🔺 The Pyramid Explained

### Level 1 — Hash Values (Trivial)
Hash values are MD5, SHA1 or SHA256 fingerprints of 
malicious files. Blocking a hash is easy for defenders 
but also trivially easy for attackers to bypass — they 
just change one byte of the file and the hash changes 
completely.

**Real world example:**  
A malware sample has hash:  
`44d88612fea8a8f36de82e1278abb02f`  
The attacker recompiles it slightly — new hash, 
same malware, bypass complete.

**SOC Analyst action:** Still worth blocking — adds 
friction even if it is not permanent.

---

### Level 2 — IP Addresses (Easy)
Blocking malicious IP addresses is slightly more 
painful for attackers but still easy to bypass using 
VPNs, proxies or bullet-proof hosting services.

**Real world example:**  
C2 server at `185.220.101.47` gets blocked.  
Attacker spins up new VPS in 10 minutes — new IP, 
same campaign.

**SOC Analyst action:** Block and monitor but do not 
rely on IPs as the primary detection mechanism.

---

### Level 3 — Domain Names (Simple)
Domain names are more painful to change than IPs 
because attackers need to register new domains, 
set up DNS and update their malware configuration.

**Real world example:**  
`evil-update-service.com` gets flagged.  
Attacker registers `windows-patch-cdn.net` — takes 
time and costs money but still doable.

**SOC Analyst action:** Monitor for domain generation 
algorithms (DGA) and newly registered domains.

---

### Level 4 — Network / Host Artefacts (Annoying)
This includes things like specific URL patterns, 
User-Agent strings, registry keys or file paths 
that malware consistently uses.

**Real world example:**  
Malware always creates a registry key at:  
`HKCU\Software\Microsoft\Windows\Update\svchost`  
Blocking this forces the attacker to rewrite their 
malware code — much more work.

**SOC Analyst action:** Build SIEM detection rules 
around consistent behavioural patterns.

---

### Level 5 — Tools (Challenging)
Tools include the actual software attackers use — 
Mimikatz, Cobalt Strike, Metasploit. Detecting and 
blocking tools forces attackers to find or build 
new ones, which takes significant time and skill.

**Real world example:**  
Organisation blocks all Cobalt Strike beacons using 
network signatures. Attacker must develop a custom 
C2 framework — weeks or months of work.

**SOC Analyst action:** Use threat intelligence feeds 
to identify known tool signatures and IOCs.

---

### Level 6 — TTPs (Tough!)
Tactics, Techniques and Procedures are the hardest 
for attackers to change because they represent HOW 
they operate — their entire methodology. This maps 
directly to the MITRE ATT&CK framework.

**Real world example:**  
An APT group always uses spearphishing with 
password-protected ZIP files as initial access 
(T1566.001). Blocking this technique forces them 
to completely change their attack methodology.

**SOC Analyst action:** Focus detection engineering 
on TTPs using MITRE ATT&CK. This is the highest 
value detection investment.

---

## 🛠️ Tools Used
- TryHackMe platform
- MITRE ATT&CK framework reference
- VirusTotal (for hash lookups)
- any.run

---

## 💡 Key Takeaways

1. **Not all IOCs are equal** — hash blocks are easy 
   to bypass, TTP detection is extremely hard to evade
   
2. **Focus detection effort high on the pyramid** — 
   the more pain you cause attackers, the better

3. **MITRE ATT&CK is the practical implementation** 
   of Pyramid of Pain at the TTP level

4. **Defence in depth** — use all levels together, 
   not just one

---

## 🔗 How This Applies to Real SOC Work

In a real SOC environment, L1 analysts primarily 
deal with the lower levels of the pyramid — blocking 
IPs and hashes from threat intel feeds. As analysts 
progress to L2/L3, they focus more on behavioural 
detection at the TTP level.

Understanding this model helps prioritise which 
alerts to focus on and which detection rules will 
give the most lasting protection against sophisticated 
attackers.

---

## 📚 References
- David Bianco — Pyramid of Pain (original paper)
- MITRE ATT&CK: https://attack.mitre.org
- TryHackMe SOC Level 1 Path
