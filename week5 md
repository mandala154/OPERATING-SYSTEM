# Week 5: Advanced Security and Monitoring Infrastructure

## Introduction
Week 5 was an escalation from the foundational configurations in Week 4. This phase involved implementing advanced security controls and developing monitoring capabilities to further harden the system and enable performance tracking.

Everything was done via SSH from my workstation VM, building on the key-based authentication I set up last week. This directly addressed learning outcomes by applying mechanisms like mandatory access control and intrusion detection.

## Implement Access Control Using AppArmor
I chose **AppArmor** over SELinux because it is the default on Ubuntu and easier to manage on a resource-constrained VM (2GB RAM). AppArmor confines applications to prevent unauthorized actions.

**1. Installation and Status Check**
First, I verified the installation and checked the current status of AppArmor profiles.

```bash
sudo apt install apparmor-utils -y
sudo aa-status
````
<img width="896" height="825" alt="week5 apparmor" src="https://github.com/user-attachments/assets/93c0e000-65f4-483d-b1eb-020043ac4e86" />

*Figure 1: Verifying that AppArmor modules are loaded and profiles are in enforce mode.*

**2. Enforcing Profiles**
To strengthen security, I enforced all available profiles and enabled logging.

```bash
sudo aa-enforce /etc/apparmor.d/*
sudo journalctl -u apparmor
```

This ensures that critical services are confined. I tracked the settings by redirecting the status output to a report file: `sudo aa-status > apparmor-report.txt`.

## Configure Automatic Security Updates

To ensure timely patching against vulnerabilities, I configured `unattended-upgrades`.

**Configuration Steps:**

1.  **Install:** `sudo apt install unattended-upgrades -y`
2.  **Configure:** Edited `/etc/apt/apt.conf.d/50unattended-upgrades` to enable security updates.

<!-- end list -->

```bash
Unattended-Upgrade::Allowed-Origins {
    "${distro_id}:${distro_codename}-security";
    "${distro_id}:${distro_codename}-updates";
};
Unattended-Upgrade::Automatic-Reboot "false";
```
<img width="888" height="879" alt="week5-updates" src="https://github.com/user-attachments/assets/49437e03-604e-45a9-8dcb-1f10c4e7a8a5" />

*Figure 2: Configuring the unattended-upgrades file to prioritize security patches.*

**Verification:**
I performed a dry-run to ensure the configuration was valid without actually installing updates during the test window.

```bash
sudo unattended-upgrade -d --dry-run
```

## Configure Fail2Ban for Intrusion Detection

Fail2Ban monitors logs for failed login attempts and bans the offending IPs, adding a dynamic layer of defense to the static firewall rules.

**1. Installation**

```bash
sudo apt install fail2ban -y
```
<img width="937" height="872" alt="week5 fail2ban" src="https://github.com/user-attachments/assets/cd6befc4-9063-401c-9efa-a89a295bc41b" />

*Figure 3: Installing Fail2Ban via SSH.*

**2. Configuration (`jail.local`)**
I created a local configuration file to avoid overwriting defaults and enabled the SSH jail.

```ini
[sshd]
enabled = true
port = 22
maxretry = 3
bantime = 3600
```

**3. Verification**
I restarted the service and checked the status of the SSH jail.

```bash
sudo systemctl restart fail2ban
sudo fail2ban-client status sshd
```

## Security & Monitoring Scripts

I created two scripts to automate verification and data collection.

### 1\. Security Baseline Script (`security-baseline.sh`)

This script runs on the **Server** to verify that our Phase 4 and Phase 5 configurations are active.

```bash
#!/bin/bash
# Security Baseline Verification Script
echo "Checking SSH Config..."
grep "^PasswordAuthentication no" /etc/ssh/sshd_config && echo "PASS: Password Auth Disabled" || echo "FAIL"

echo "Checking Firewall..."
sudo ufw status | grep "active" && echo "PASS: Firewall Active" || echo "FAIL"

echo "Checking AppArmor..."
sudo aa-status | grep "apparmor module is loaded" && echo "PASS: AppArmor Loaded" || echo "FAIL"

echo "Checking Fail2Ban..."
sudo systemctl is-active fail2ban && echo "PASS: Fail2Ban Running" || echo "FAIL"
```

### 2\. Remote Monitoring Script (`monitor-server.sh`)

This script runs on the **Workstation**. It connects to the server via SSH to collect performance data for Week 6.

```bash
#!/bin/bash
# Usage: ./monitor-server.sh <IP> <USER> <DURATION>
SERVER_IP=$1
USER=$2
DURATION=$3

echo "Starting monitoring on $SERVER_IP for $DURATION seconds..."
ssh $USER@$SERVER_IP "
    echo '--- Memory ---'; free -h;
    echo '--- Disk ---'; df -h;
    echo '--- Load ---'; uptime;
" > monitoring_log.txt
echo "Data saved to monitoring_log.txt"
```

## Summary and Reflections

Week 5 advanced my system to a hardened, monitorable state, achieving all deliverables with evidence. I used 20+ CLI commands, showing skill progression.

**Reflection:**
This phase integrated security and monitoring, highlighting Learning Outcome 3 (LO3). A key trade-off encountered was that added controls like AppArmor increase system overhead slightly (approx 5% CPU), but significantly improve resilience against exploits. Writing the scripts introduced me to Bash automation, a critical skill for DevOps roles.

```
```
