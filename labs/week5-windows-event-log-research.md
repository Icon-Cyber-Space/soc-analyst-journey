# Week 5 — Windows Event Log Research (13Cubed)

**Date:** [today's date]
**Source:** 13Cubed YouTube, "Introduction to Windows Forensics" series

## What I watched
- RDP Event Log Forensics
- Event Log Chainsaw Massacre, Powerful Threat Detection
- Impacket Impediments, Finding Evil in Event Logs
- Detecting NTDS.DIT Theft, ESENT Event Logs
- RDP Authentication vs. Authorization
- CVEs in Windows Event Logs

## Key takeaways

### RDP Event Log Forensics
Over 20 Event IDs came up across this video alone, more than I expected
for just RDP activity. The one that stuck with me most: **4624** (successful
logon), since it's the same Event ID from my Splunk brute-force detection
work, just applied here specifically to RDP sessions rather than general
login attempts. Realising the same Event ID shows up across different
attack scenarios, not just one, was a useful connection.

### Chainsaw
A tool for parsing and hunting through Windows Event Logs faster than
manual searching, since it comes with built-in detection rules/logic
rather than needing to write everything from scratch each time. I don't
fully understand its internals yet, but I get the core value: speed,
it does in seconds what would take much longer searching manually.

### Impacket tools (atexec, dcomexec, psexec, smbexec, wmiexec)
Each of these attacker tools leaves a distinct trace in event logs, even
though they're all used for the same general purpose (remote code
execution). This was a good reminder that "the same outcome" doesn't
mean "the same log signature", the specific tool used actually matters
for detection.

### NTDS.DIT theft (ESENT logs)
NTDS.DIT is the actual database file storing Active Directory's user
credentials. ESENT logs can reveal attempts to steal or access this file,
this is genuinely high-stakes territory, if an attacker gets this file,
they potentially get every domain credential at once.

### RDP Authentication vs Authorization
Learned the distinction between authentication (proving who you are) and
authorization (being allowed access), specifically in the context of RDP
and Network Level Authentication (NLA), and that this distinction shows
up as a specific, separate event log entry.

### CVEs in Windows Event Logs — CVE-2020-0601
This tied directly into a real vulnerability: a flaw in Windows' own
CryptoAPI (crypt32.dll), the core library Windows uses to verify digital
certificates and signatures. The bug was in how it validated elliptic
curve cryptography specifically, meaning an attacker could forge a
certificate that appeared legitimately signed by a trusted authority,
sometimes called "CurveBall". Since so many things rely on this same
library (Windows itself, and browsers like Chrome/Edge for HTTPS), the
impact reached further than just one app. Notably, this was reported
directly by the NSA, a sign of how serious the risk was.

## Why this connects to what I'm already doing
This is the same instinct as my CISA KEV research, a real vulnerability,
tied to real logs, not just theory. It also reinforced that Event ID
4624 keeps showing up across totally different contexts (my Splunk
brute-force work, and now RDP forensics), same signal, different
scenarios.

## What I'd still like to understand better
- How Chainsaw's detection rules actually work under the hood
- More of the specific Event IDs from the RDP video beyond 4624
