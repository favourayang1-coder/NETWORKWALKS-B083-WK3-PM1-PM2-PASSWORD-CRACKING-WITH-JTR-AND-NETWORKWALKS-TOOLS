# Password Cracking Lab — JTR & NetworkWalks Tools

## Overview

This repo covers **Week 3** of my Cybersecurity & Ethical Hacking training at NetworkWalks Academy, spanning **Project Module 1** and **Project Module 2**.

Both modules had the same end goal — recover the password protecting an encrypted PDF — but approached it two different ways:

1. **John the Ripper**, run from the terminal on Kali Linux — the classic, long-standing command-line password auditing tool.
2. **NetworkWalks' Hash Calculator and Password Cracker** — a pair of free browser tools that do the same job with zero setup.

Working through both gave me a good side-by-side look at a CLI-driven workflow versus an entirely browser-based one, using identical logic underneath.

**Modules covered:**
- W3-PM1 — Password Cracking with JTR
- W3-PM2 — Password Cracking with NetworkWalks Tools

## What I Set Out to Do

- Pull a crackable hash out of a password-protected PDF
- Run a dictionary attack against that hash with John the Ripper
- Repeat the same recovery using NetworkWalks' browser tools instead
- Confirm the recovered password actually opens the file
- Compare how the two methods felt to use in practice

## Environment

| Item | Detail |
|---|---|
| OS | Kali Linux |
| CLI Tool | John the Ripper (pre-installed) |
| Hash extraction (CLI) | `pdf2john` |
| Browser tool 1 | [NetworkWalks Hash Calculator](https://networkwalks.com/hash-calculator/) |
| Browser tool 2 | [NetworkWalks Password Cracker](https://networkwalks.com/password-cracker/) |
| Target file | My Locked PDF1.pdf |

## How the Two Paths Compare at a Glance

```
Locked PDF
   |
   |-- pdf2john (CLI) --> hash.txt --> john hash.txt --> password found
   |
   |-- NetworkWalks Hash Calculator (browser) --> $pdf$ hash --> NetworkWalks Password Cracker --> password found
```

Same starting point, same end result, two different routes to get there.

## Module 1 — Cracking It with John the Ripper

Since JTR ships with Kali by default, this one stayed entirely in the terminal.

**What I did:**
1. Moved into the folder holding the target PDF (`cd /home/kali/Downloads`).
2. Extracted the hash with `pdf2john`:
   ```
   pdf2john "My Locked PDF1.pdf" > hash.txt
   ```
   (Had to quote the filename since it contains spaces — without the quotes the shell tried to read it as separate arguments and couldn't find the file.)
3. Checked `hash.txt` and confirmed it came out clean, in the `$pdf$...` format John expects.
4. Ran the attack:
   ```
   john hash.txt
   ```
   It cracked almost instantly against the default wordlist.
5. Confirmed the result with:
   ```
   john --show hash.txt
   ```
6. Opened the PDF using the password it found and confirmed it unlocked.

**Password recovered:** `good-luck`

## Module 2 — Cracking It Again with NetworkWalks' Tools

Same target file, same goal, but this time entirely through the browser.

**What I did:**
1. Opened the NetworkWalks Hash Calculator and switched to its PDF mode.
2. Uploaded the same locked PDF — everything runs client-side in the browser, so the file itself never leaves my machine.
3. Copied the full `$pdf$...` hash it generated.
4. Opened the NetworkWalks Password Cracker in a separate tab and pasted the hash in.
5. Left it on the built-in wordlist and hit Start Cracking.
6. Watched it work through the list until it landed on a match.
7. Opened the PDF again with the recovered password to double-check it was correct.

**Password recovered:** `good-luck` — identical to Module 1.

## Results

| File | Method | Outcome |
|---|---|---|
| My Locked PDF1.pdf | John the Ripper (CLI) | Password recovered — `good-luck` |
| My Locked PDF1.pdf | NetworkWalks Hash Calculator + Password Cracker | Password recovered — `good-luck` |

## CLI vs. Browser: My Take

| | John the Ripper | NetworkWalks Tools |
|---|---|---|
| Getting started | Already installed on Kali | Nothing to install, just open a browser tab |
| Extracting the hash | `pdf2john` script | Hash Calculator's PDF mode |
| Running the attack | Wordlist attack via John's engine | Wordlist attack via the built-in list |
| Watching it work | Mostly quiet until it finds a match | Shows each attempt as it goes, easier to follow along |
| Where it fits | Feels more suited to serious, repeatable pentest work | Feels better for quick checks or just learning the concept |

Landing on the exact same password both times was a good reminder that the tool is really just the delivery mechanism — the actual attack (pull the hash, try candidates, compare, report a match) is the same idea whether it's running in a terminal window or a browser tab.

## What I Took Away From This

Doing this twice back-to-back made the theory click in a way just reading about it wouldn't have. A password like `good-luck` doesn't stand much of a chance against either a proper wordlist tool or a lightweight browser one — both got there in seconds. It also reframed how I think about a "hash" — it's not really protection on its own, it's just a scrambled version of the password that's only as safe as the password itself is hard to guess.

## Scope & Ethics

Everything here was done against a file provided specifically for this NetworkWalks training exercise, inside a controlled lab setup. None of the tools or steps documented here should be pointed at a file or system without clear authorization to do so.

## Project Info

**Program:** Cybersecurity & Ethical Hacking — NetworkWalks Academy
**Modules:** W3-PM1 (Password Cracking with JTR) & W3-PM2 (Password Cracking with NetworkWalks Tools)

## Author
*[Your name]*
Cybersecurity & Ethical Hacking Trainee
