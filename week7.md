# Week 7: Security Audit and System Evaluation

## Introduction
Week 7 was the final and most reflective phase of this coursework, where I conducted a comprehensive security audit and overall system evaluation to validate the configurations from Weeks 4-6. This phase involved mandatory tasks like Lynis scanning, network assessment with nmap, access control verification, and a service audit.

It provided an opportunity to critically assess the effectiveness of my security implementations and identify any remaining risks, directly contributing to learning outcomes by evaluating vulnerabilities and threats.

## Infrastructure Security Assessment
I reviewed the overall setup for compliance with my Week 4-5 configurations. Using my `security-baseline.sh` script, I confirmed that all checks passed (SSH hardened, firewall active). The infrastructure is secure for an isolated educational environment, with no exposed ports beyond SSH and strong user controls.

## Lynis Security Audit
Lynis is an open-source auditing tool for Linux security. I installed it via SSH and ran the initial audit.

**1. Installation and Execution**
```bash
sudo apt install lynis -y
sudo lynis audit system
````
<img width="948" height="857" alt="week7-audit" src="https://github.com/user-attachments/assets/34fc65d0-400e-488b-9c3f-25c9465beb7c" />

*Figure 1: Executing the Lynis system audit to identify hardening opportunities.*

**2. Scores and Remediation**

  * **Initial Score:** 72/100
  * **Issues Found:** Outdated packages, unused services (CUPS), and loose file permissions.
  * **Remediation Actions:**
      * Updated all packages: `sudo apt update && sudo apt upgrade -y`
      * Disabled printing service: `sudo systemctl disable cups`
      * Tightened permissions: `sudo chmod 640 /etc/shadow`
  * **Final Score:** 88/100 (A 16% improvement, exceeding the target of \>80).

## Network Security Testing

I used `nmap` for a local network assessment to verify that my firewall rules were effective.

```bash
sudo nmap -sV localhost
```

**Results:**

  * **Open Ports:** Only port 22/tcp (OpenSSH 9.2p1).
  * **Vulnerabilities:** None detected.
  * **Conclusion:** The host-only network isolation and UFW rules successfully block all unauthorized ingress traffic.

## SSH Security Verification

I verified the SSH configuration from the workstation to ensure hardening was active.

```bash
ssh -v admin@192.168.56.10
```

**Observations:**

  * Connection succeeded using the ED25519 key.
  * Password authentication was not offered (as expected).
  * Fail2Ban logs (`sudo journalctl -u fail2ban`) showed no unauthorized attempts, confirming the idle state of the isolated network.

## Service Inventory

I audited the running services to ensure only essential processes were active.

**Command:** `systemctl list-units --type=service --state=running`

| Service | Status | Justification |
| :--- | :--- | :--- |
| `sshd.service` | Active | Essential for remote administration. |
| `ufw.service` | Active | Critical for network security. |
| `apparmor.service` | Active | Mandatory Access Control. |
| `fail2ban.service` | Active | Intrusion detection system. |
| `unattended-upgrades` | Active | Automates security patching. |

I disabled unnecessary services such as `avahi-daemon` (mDNS) and `bluetooth.service` to reduce the attack surface.

## Remaining Risk Assessment

  * **Overall Risk:** Low.
  * **Mitigations:** No external exposure (host-only network), high Lynis score, and all threats from Week 2 mitigated.
  * **Residual Risks:**
      * **VM Host Vulnerabilities:** Mitigated by keeping the host OS updated.
      * **Human Error:** Mitigated by using strong passphrases for SSH keys.

## Summary and Reflections

Week 7 completed the coursework with a thorough audit, confirming my system's security and readiness. I achieved all mandatory tasks, with quantitative improvements like the Lynis score increase from 72 to 88.

**Reflection:**
This phase was insightful, showing how configurations integrate to form a secure whole. Auditing tools like Lynis add overhead but reveal weaknesses that manual checks might miss. The main challenge was interpreting the specific Nmap flags, which I overcame by consulting the man pages. Overall, the 7 weeks transformed my Linux knowledge, and I am confident in the system's security posture.
