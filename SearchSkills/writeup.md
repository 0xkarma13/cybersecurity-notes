# TryHackMe — Search Skills Room

In this room I went through a few well-known, useful sites for gathering information in cybersecurity — tools that are handy for both red team and blue team work. They help with finding exploits, gathering more information, understanding how tools actually work, and — most importantly — knowing what to search for and why.

## Shodan

Shodan is known as a search engine for the internet, but it's more than that. It continuously scans the internet looking for open ports, systems running default passwords, traffic cameras, and even industrial control systems — basically anything connected to the public network — so you can see what's running where.

For example, searching `apache 2.4.1` returns a list of services running that exact version. Results are organized by country, organization, and port, which is genuinely useful during a pentest.

Useful filters:
- `country:IE` — narrow results down to a specific country
- `port:22` — filter by a specific port number
- `hostname:fakebank.thm` — search by a specific hostname or domain

TryHackMe has a separate room dedicated to Shodan, which I'll tackle and write up later.

## VirusTotal

VirusTotal aggregates results from around 70 antivirus engines and scanners into a single interface. You give it a file, a domain, or even a hash, and it tells you whether any of those 70 engines flagged it as malicious. It's not perfect, but it's a popular tool among blue teamers for getting a quick community consensus on suspicious files and links.

## CVE (Common Vulnerabilities and Exposures)

This is about the closest thing the security industry has to a unified dictionary of known vulnerabilities. Every confirmed vulnerability gets a unique identifier in the format `CVE-YEAR-NUMBER` (e.g. `CVE-2025-55182`). You've probably heard names like Heartbleed, React2Shell, and Log4Shell — those map back to CVEs.

Each one is scored (CVSS) based on factors like:
- **Impact** — how much damage this vulnerability could cause
- **Complexity** — how easy or hard it is to exploit
- **Availability** — how likely it is that someone could actually exploit it

These IDs act as a common reference point between vendors, pentesters, and the wider security community — making sure that when someone talks about a vulnerability, everyone knows exactly which one they mean.

## The `man` command

Every major security tool or platform ships its own official documentation, and that's more accurate, principled, and trustworthy than any outside tutorial. When you don't know how to use a tool, the first place to check is its own official docs — not the last resort.

The `man` command lets you see and learn from the useful information the tool's own creators put together for it.

## GitHub

GitHub can be a great resource because researchers often publish proof-of-concept (PoC) code, exploit tools, and detailed technical write-ups there — usually faster than through official channels.

That said, not every PoC is accurate or trustworthy: some are incomplete, some are intentionally flawed, and sometimes the repo itself contains a PoC that's actually malicious. So before using one, it needs to be carefully reviewed and verified.
