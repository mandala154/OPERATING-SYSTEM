# Week 4: Initial System Configuration & Security Implementation

## Introduction
Week 4 marked the transition from planning to practical implementation in this coursework. Building on the system architecture I designed in Week 1 and the security planning from Week 2, I focused on deploying foundational security controls on my Linux server VM.

The brief emphasizes that all configurations must be performed via SSH from the workstation, which enforced my command-line proficiency and helped me develop remote administration skills. This phase was crucial for assessing and applying security mechanisms to protect against exploits.

I used my VirtualBox setup: the server VM running Ubuntu Server 24.04 LTS (headless, IP 192.168.56.10) and the workstation VM running Ubuntu Desktop 24.04 LTS (IP 192.168.56.11), connected via host-only network.

## Configure SSH with Key-Based Authentication
To secure remote access, I implemented key-based authentication, disabling password login to mitigate brute-force threats as planned in Week 2.

**1. Generate Key Pair on Workstation**
On the workstation terminal, I generated an ED25519 key pair because it's more secure and efficient than RSA.

```bash
ssh-keygen -t ed25519 -C "admin@workstation"
````
<img width="905" height="786" alt="week4 ssh-keygen" src="https://github.com/user-attachments/assets/e9e3482c-e5a0-431f-8f09-a7f48906efa8" />

*Figure 1: Generating the ED25519 SSH key pair on the workstation.*

**2. Copy Public Key to Server**
I copied the public key to the server using `ssh-copy-id`. This added the key to `~/.ssh/authorized_keys` on the server.

```bash
ssh-copy-id admin@192.168.56.10
```

**3. Harden SSH Configuration**
I SSH'ed to the server and edited the configuration file (`/etc/ssh/sshd_config`) to disable insecure features.

**Configuration Changes:**

  * `PubkeyAuthentication yes` (Enables keys)
  * `PasswordAuthentication no` (Disables passwords)
  * `PermitRootLogin no` (Prevents root login)
  * `MaxAuthTries 3` (Limits attempts)

I verified the setup by restarting the service (`sudo systemctl restart sshd`) and logging in without a password.

## Configure a Firewall Permitting SSH from One Specific Workstation Only

Following the Week 2 checklist, I configured UFW (Uncomplicated Firewall) to restrict access.

**1. Install and Set Default Policies**

```bash
sudo apt update && sudo apt install ufw -y
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

**2. Allow Specific SSH Access**
I allowed SSH connections *only* from my workstation's specific IP address, limiting the attack surface.

```bash
sudo ufw allow from 192.168.56.11 to any port 22 proto tcp
sudo ufw enable
```
<img width="953" height="640" alt="week4-firewall-proof" src="https://github.com/user-attachments/assets/8e31e98b-a031-4de2-8849-3bf96256eadb" />

*Figure 2: Verifying the UFW status and ruleset.*

**Firewall Documentation Showing Complete Ruleset:**

```text
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), deny (routed)

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    192.168.56.11
```

## Manage Users and Implement Privilege Management

To enforce least privilege, I created a non-root administrative user.

**Steps Taken:**

1.  **Create User:** `sudo adduser admin`
2.  **Grant Sudo:** `sudo usermod -aG sudo admin`
3.  **Sudo Timeout:** Edited `sudo visudo` to add `Defaults env_reset,timestamp_timeout=15`.
4.  **Lock Root:** `sudo passwd -l root`

This prevents direct root access, reducing escalation risks.

## Configuration Files with Before and After Comparisons

I documented the changes to `sshd_config` by backing up the original file and running a diff comparison.

**Diff Output Summary:**

```diff
< PasswordAuthentication yes
---
> PasswordAuthentication no
< PermitRootLogin yes
---
> PermitRootLogin no
```

## Remote Administration Evidence

To demonstrate remote administration proficiency, I executed several system commands via SSH from the workstation.

```bash
uptime
free -h
df -h
sudo whoami
```
<img width="952" height="648" alt="week 4 remote command" src="https://github.com/user-attachments/assets/30f274f9-87ac-46a7-83e6-856ac5a5499e" />

*Figure 3: Executing administrative commands remotely via SSH.*

## Summary and Reflections

Week 4 was pivotal, turning plans into a secure system. I achieved all deliverables, with quantitative evidence like diff comparisons.

**Reflection:**
The process highlighted trade-offs—hardening improves security but requires careful testing to avoid lockouts. I used 15+ CLI commands, showing progression from Week 1 basics. A misconfigured rule locked me out once, but console access fixed it, teaching backup importance. This develops my DevOps skills for real-world roles.
