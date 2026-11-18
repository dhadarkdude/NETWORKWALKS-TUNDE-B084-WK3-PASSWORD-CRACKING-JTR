# NETWORKWALKS-TUNDE-B084-WK3-PASSWORD-CRACKING-JTR
# Password Cracking with John the Ripper (NetworkWalks Week 3 Project)

A hands-on password-cracking exercise completed as part of the NetworkWalks Cybersecurity and Ethical Hacking internship, Week 3. Covers extracting a crackable hash from a password-protected PDF and recovering the password two ways: offline with John the Ripper, and through NetworkWalks' own browser-based dictionary-attack simulator, to compare how fast weak vs. stronger-looking passwords actually fall.

## Project Overview

NetworkWalks provided three password-locked PDFs, each hiding a flag that only unlocks once the password is recovered. The goal wasn't just to get the flags, it was to see, with real numbers, how little a password's appearance of complexity matters if it's predictable. Every PDF in this lab was cracked in well under a second against a real breach wordlist, including the one that looked the "strongest."

## Objectives

- Extract a crackable hash from an encrypted PDF using `pdf2john`
- Crack the hash offline with John the Ripper against the `rockyou.txt` wordlist
- Cross-check the same hash using NetworkWalks' Hash Calculator and Password Cracker (Dictionary Attack) tools
- Compare crack time/attempts across passwords of different apparent complexity
- Draw a real conclusion about what actually makes a password strong

## Lab Environment

- **OS:** Kali Linux (VMware Workstation VM)
- **Tools:** `pdf2john` (part of the John the Ripper suite), John the Ripper, `rockyou.txt` wordlist
- **Target files:** `My-Locked-PDF1.pdf`, `My-Locked-PDF2.pdf`, `My-Locked-PDF3.pdf` (provided by NetworkWalks)
- **Secondary tools:** [NetworkWalks Hash Calculator](https://networkwalks.com/hash-calculator/) and [NetworkWalks Password Cracker through Dictionary Attacks](https://networkwalks.com/password-cracker/) — browser-based tools that extract a `$pdf$` hash from an encrypted PDF (parsed locally, nothing uploaded) and run a visible dictionary attack against it

## Lab Setup Procedure

### PDF1

**1. Extract the hash:**
```bash
pdf2john My-Locked-PDF1.pdf > pdf1hash.txt
```
![PDF1 hash extraction](./hashing_pdf1.png)

**2. Crack offline with John the Ripper:**
![PDF1 cracked with John the Ripper](./pdf1_cracked.png)

**3. Cross-check with NetworkWalks Hash Calculator:**
![PDF1 hash via NetworkWalks Hash Calculator](./pdf1_NW_hash_calc.png)

**4. Cross-check with NetworkWalks Password Cracker (dictionary attack):**
![PDF1 cracked via NetworkWalks Password Cracker](./pdf1_NW_password_cracked.png)

**5. Unlock the PDF and capture the flag:**
![PDF1 flag captured](./pdf1_success.png)

### PDF2

**1. Extract the hash:**
![PDF2 hash extraction](./pdf2_hashed.png)

**2. Crack offline with John the Ripper:**
![PDF2 cracked with John the Ripper](./pdf2_cracked.png)

**3. Cross-check with NetworkWalks Hash Calculator:**
![PDF2 hash via NetworkWalks Hash Calculator](./pdf2_NW_hash_calc.png)

**4. Cross-check with NetworkWalks Password Cracker (dictionary attack):**
![PDF2 cracked via NetworkWalks Password Cracker](./pdf2_NW_pass_cracked.png)

**5. Unlock the PDF and capture the flag:**
![PDF2 flag captured](./pdf2_success.png)

### PDF3

**1. Extract the hash:**
![PDF3 hash extraction](./pdf3_hashed.png)

**2. Crack offline with John the Ripper:**
![PDF3 cracked with John the Ripper](./pdf3_cracked.png)

**3. Cross-check with NetworkWalks Hash Calculator:**
![PDF3 hash via NetworkWalks Hash Calculator](./pdf3_NW_hash_calc.png)

**4. Cross-check with NetworkWalks Password Cracker (dictionary attack):**
![PDF3 cracked via NetworkWalks Password Cracker](./pdf3_NW_pass_cracked.png)

**5. Unlock the PDF and capture the flag:**
![PDF3 flag captured](./pdf3_success.png)

### All Three Passwords, Side by Side

![All three cracked passwords compared](./all_passwords.png)

## Lab Verification

| File | Extracted Hash | Cracked Password | Offline (John + rockyou.txt) | Online dictionary tool | Flag Captured |
|---|---|---|---|---|---|
| My-Locked-PDF1.pdf | `$pdf$4*4*128*-1060*1*16*55d1a5c14175da449753199e44971d32*32*777fd021a7f3c5ae598c0c6495c7f76e00000000000000000000000000000000*32*ceecdac74b19b5a62688d3b3524e1374c955cbb9cc3c45316494d9446ef81af1` | `password1` | Instant (0:00:00:00) | Matched after 15 of 3,556 guesses | `nw{networkwalks_flag1_jtr_270521_1}` |
| My-Locked-PDF2.pdf | `$pdf$4*4*128*-1028*1*16*0853f2cde0ef15b1c0f93ed229d3b1ad*32*8f13ce5aa39ad974364d36a057da76790021446990b9e4114071a4d9104984c1*32*ceecdac74b19b5a62688d3b3524e1374c955cbb9cc3c45316494d9446ef81af1` | `password1` | Instant (0:00:00:00) | Not re-run (same password as PDF1) | `nw{networkwalks_persistence_jtr_270521}` |
| My-Locked-PDF3.pdf | `$pdf$4*4*128*-1028*1*16*34eb542eff4e1b0b32d25ce15a9a7281*32*b77872bfc9a24fb2f845066283a8fc1b0021446990b9e4114071a4d9104984c1*32*e7572256e4b552cd57988f5134214b91920d94d7a6bf550ea94a2995c7f2ab02` | `1qaz2wsx` | Instant (0:00:00:00) | Matched after 2,078 of 3,556 guesses (58%) | `nw{networkwalks_flag_260821_1}` |

**The key result:** `1qaz2wsx` looks more complex on the surface than `password1`, mixed letters and numbers, 8 characters, but it's a well-known keyboard-walk pattern (literally walking across two rows of a QWERTY keyboard), one of the most common entries in any real breach wordlist. It still fell instantly offline against `rockyou.txt`. The only reason it took longer in the *online* tool is that the simulator works through its list in frequency order and this pattern sits further down a shorter demo list than "password" does. Against a real 14-million-entry list, the gap barely matters.

## Problems Encountered & Solutions

No major blockers in this one — the main thing to get right was recognizing the `$pdf$...` hash format John and the NetworkWalks Hash Calculator both produced, so I could confirm both tools were genuinely working from the same hash rather than two unrelated outputs. Filenames with spaces in the earlier Hash Calculator screenshots were also a reminder to always quote paths when scripting around files you didn't name yourself.

## What I Learned

- **"Complex-looking" is not the same as "strong."** `1qaz2wsx` has letters, numbers, and 8 characters, every box a typical "password requirements" policy checks, and it still fell as fast as `password1`, because it's a predictable keyboard-walk pattern.
- **Password cracking is a hashing + lookup problem, not a brute-force-everything problem.** Most real-world password attacks never need to brute-force random character space, they just walk a wordlist built from previous breaches, because most people reuse predictable patterns.
- **The defense is unpredictability, not just length.** A long, truly random passphrase has no shortcut into a wordlist attack, it forces the attacker into brute force, which is computationally a different universe of difficulty.
- **This is why password managers and passphrases matter in everyday life**, not just in a lab. The three PDFs in this exercise are stand-ins for every account that gets "secured" with a password that's long enough to feel safe but common enough to be in the first file an attacker tries.

## Security & Ethical Use

This exercise was performed entirely within the NetworkWalks lab environment on test files and dummy credentials provided by NetworkWalks specifically for this exercise, not real-world systems or accounts. Hashes, cracked passwords, and flags shown above are from that controlled lab only.

## Tools & Resources

- [John the Ripper](https://www.openwall.com/john/)
- `pdf2john` (bundled with John the Ripper)
- `rockyou.txt` wordlist
- [NetworkWalks Hash Calculator](https://networkwalks.com/hash-calculator/)
- [NetworkWalks Password Cracker through Dictionary Attacks](https://networkwalks.com/password-cracker/)

## Author

Tunde Oduola
[LinkedIn](https://www.linkedin.com/in/tunde-oduola-2855a5239)

## Project Information

- **Internship/Program:** NetworkWalks, Cybersecurity and Ethical Hacking Class
- **Week:** 3
- **Project:** Password Cracking with John the Ripper
- **Organization:** NetworkWalks
- **Repository Name:** NETWORKWALKS-TUNDE-B084-WK3-PASSWORD-CRACKING-JTR
- **Repository Type:** Public