# Windows Event IDs — RDP Reference

Notes from 13Cubed's Windows Forensics series (RDP Event Log Forensics,
RDP Authentication vs Authorization).

| Event ID | Log Source | Meaning |
|---|---|---|
| 21 | TerminalServices-LocalSessionManager | RDP session logon succeeded |
| 22 | TerminalServices-LocalSessionManager | Shell start notification (desktop session started) |
| 1149 | TerminalServices-RemoteConnectionManager | NLA authentication succeeded (fires before full session established) |

## Key distinction: authentication vs authorization
- **1149** confirms authentication (proving who you are) succeeded.
- **21** confirms the session was actually logged in and established.
- Seeing 1149 alone doesn't guarantee a session was created, both should
  be checked together to understand what actually happened.

## Also relevant (from Splunk work, Week 5-6)
| Event ID | Meaning |
|---|---|
| 4624 | Successful logon (general, not RDP-specific) |
| 4625 | Failed logon (used for brute-force detection) |
