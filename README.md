# Operation Catch the Hacker

A full red team vs. blue team cybersecurity exercise completed as a capstone project for the Fullstack Academy Cybersecurity program (June–July 2026). A four-person team ran a complete attack-detect-defend cycle against an isolated lab network of two Kali Linux VMs — no real systems, no real data, no real targets.

## My Role: Blue Team Detection Analyst

I received the attacker's packet capture and worked backward through it to find, confirm, and document the attack — then handed off actionable detection artifacts to the defense team.

**What I did:**
- Analyzed a 2,245-packet Wireshark capture (`capture.pcapng`) of an SSH brute-force attack
- Isolated the Nmap reconnaissance phase using a SYN-packet filter (2,092 packets identified)
- Isolated the SSH brute-force phase using a port filter (181 packets identified)
- Identified two Hydra tool fingerprints in the packet data: repeated high-numbered ephemeral source ports and the `SSH-2.0-libssh_0.11.2` client banner
- Wrote and validated a Snort 3 detection rule targeting the attack
- Used `tcpdump` as a fallback evidence tool after hitting a known Snort 3.x segmentation fault bug, confirming findings via packet statistics and raw output instead
- Produced a full findings write-up with severity ratings and a Blue Team handoff package

**Tools:** Wireshark, Snort 3, tcpdump, Kali Linux

## Full Team Cycle

| Phase | Owner | Outcome |
|---|---|---|
| Attack | Red Team | SSH brute force succeeded in under 1 minute (Nmap + Hydra) |
| Phishing | Red Team | 3 annotated AI-assisted phishing samples + red-flag guide |
| **Detection** | **Blue Team (me)** | **Attack found in packets, Snort rule written and validated** |
| Defense | Blue Team | Firewall (UFW) blocked the attack; phishing awareness guide delivered |

## Repo Contents

```
reports/
├── 00-project-summary.pdf        Full team overview: all 4 roles + outcome
├── 01-red-team-attack.pdf        Nmap + Hydra SSH brute force walkthrough
├── 02-red-team-phishing.pdf      Recon + 3 annotated phishing samples
├── 03-blue-team-detection.pdf    My detection analysis (Wireshark, Snort, tcpdump)
└── 04-blue-team-defense.pdf      UFW firewall hardening + phishing guide
detection/
└── local.rules                   Snort rule referenced in my detection report
```

## Key Detection Finding

```
alert tcp any any -> any 22 (msg:"SSH login attempts, possible brute force"; sid:1000001; rev:1;)
```

This rule loaded successfully against the full capture and would fire on equivalent brute-force traffic in a real SOC environment.

## Disclaimer

This project was conducted entirely within an authorized, isolated lab environment for educational purposes. All IP addresses, phishing samples, and target systems are fictional or lab-only. No real systems, companies, or individuals were targeted.
