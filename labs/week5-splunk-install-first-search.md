# Splunk Install + First Search (WSL Ubuntu)

**Platform:** Splunk Enterprise (Free license)
**Environment:** WSL Ubuntu
**Date:** [today's date]

## What I set out to do
Install Splunk on WSL, get it running, and run my first real search
against actual log data, not a tutorial video, an actual working
installation on my own machine.

## The install
Downloaded the .deb package directly from Splunk's site using wget,
then installed it with:
sudo dpkg -i splunk-10.4.3-4174a2deda5d-linux-amd64.deb
Straightforward, no issues here. The install unpacked cleanly, one
harmless warning about a missing python3.7 folder that didn't affect
anything.

## Where it actually got hard: starting it for the first time
This is where most of my time went, not the install itself, but getting
Splunk to actually start and stay usable.

1. First attempt got stuck in Splunk's license agreement pager, kept
   asking to press Space or Enter and wouldn't respond. Had to force-kill
   the process with `sudo pkill -f splunk` from a second terminal window.
2. Second attempt failed because Splunk refuses to run as root by default,
   needed to explicitly add `--run-as-root` since I was using sudo.
3. Once it finally started, I hit a bigger issue: my trial license showed
   an expiration date that had already passed, even though I'd just
   installed it. Turns out this is a known Splunk issue, trial packages
   sometimes ship with a fixed expiration baked in rather than one based
   on install date.
4. Tried switching to the Free license through the web interface, it kept
   bouncing back to the same selection screen without saving.
5. Had to bypass the web UI entirely and force the license switch directly
   from the command line:
sudo /opt/splunk/bin/splunk edit licenser-groups Free -is_active 1 -auth admin:mypassword
6. After that, got a new problem, a login/logout loop, then a "Login
   failed, your license is expired" lockout, a genuine chicken-and-egg
   problem since I couldn't log in to fix the license, and couldn't fix
   the license without logging in.
7. Fixed it by stopping Splunk, re-running the license edit command, and
   restarting.
8. Even after the server was actually fixed, my regular Chrome window
   kept showing the old broken state because of cached site data. Had to
   clear it properly through Chrome DevTools (Application tab, Clear site
   data), a basic cookie clear wasn't enough.

## What I actually learned from all of this
- `sudo` matters for more than just "run this command", processes started
  with sudo need sudo again to be killed or modified
- Software licensing has its own state, separate from just "is the
  program installed", and that state can get stuck in ways that need
  fixing directly through the command line, not just the UI
- Browsers cache more than I realized, cookies alone weren't enough to
  clear a stuck session, had to clear localStorage/sessionStorage too
- When a web UI won't cooperate, checking if the tool has a CLI equivalent
  is often the fastest way through, `splunk edit licenser-groups` and
  `splunk list licenser-groups` bypassed the browser entirely, both to
  cause and fix the problem

## The actual first search
Once everything was finally stable, ran this in Search & Reporting:
index=_internal | head 10

**What it does:** `index=_internal` filters to Splunk's own internal
logs (always exists by default, no setup needed). The pipe (`|`) sends
that output into `head 10`, which limits the display to just the first
10 results instead of everything.

**What came back:** real internal Splunk log entries, including one that
was literally Splunk logging the HTTP request my own search had just
made. Saw the three fields every Splunk log has: `host` (which machine),
`source` (which file the log came from), and `sourcetype` (what kind of
log it is).

## What I'd do differently
Next time, I'd check for known installation issues (like the trial
license expiration bug) before starting, rather than debugging it fresh.
Would've saved real time.

## Why this matters going forward
This whole session was less about learning SPL syntax and more about
real troubleshooting, working through stuck processes, permission
issues, and cached state, skills that come up constantly in any
technical role, not just SOC analyst work specifically.
