# Week 1: System Planning and Distribution Selection

## Introduction
Week 1 marked the official start of the coursework. This week was entirely about planning and justification (Phase 1 of the brief) and required no actual configuration changes.

The deliverables were:
* A clear system architecture diagram
* Distribution selection with justification
* Workstation configuration decision
* Network configuration documentation
* CLI-gathered system specifications

I spent approximately 5 hours on research, creating the diagram, and running the required commands directly on the server console (no SSH yet).

## System Architecture Diagram
I created the diagram using draw.io and exported it as a high-resolution PNG. The diagram clearly shows the server VM (Ubuntu Server), the workstation VM (Ubuntu Desktop), and the host-only network connecting them.

![System Architecture Diagram](<img width="1536" height="1024" alt="week1 png" src="https://github.com/user-attachments/assets/f6631e64-c60f-462a-9700-2e7249e26ba8" />

)

**Key Components:**
* **Server VM:** Ubuntu Server 24.04 LTS (headless)
* **Workstation VM:** Ubuntu Desktop 24.04 LTS
* **Network:** VirtualBox host-only network (`vboxnet0`)
* **IP Addresses:** Server `192.168.56.10` / Workstation `192.168.56.11`

## Distribution Selection Justification
After comparing several distributions, I selected **Ubuntu Server 24.04 LTS** for the following reasons:

| Distribution | Pros | Cons | Chosen? |
| :--- | :--- | :--- | :--- |
| **Ubuntu Server 24.04** | Huge community, excellent documentation, LTS support until 2029, AppArmor by default, easy package management | Slightly larger attack surface than minimal distros | **YES** |
| **Debian 12** | Extremely stable, conservative updates | Older packages, slower security patches | No |
| **Rocky Linux 9** | RHEL-compatible, enterprise focus | Smaller community for desktop use | No |
| **Arch Linux** | Bleeding-edge, fully customisable | Rolling release, high maintenance | No |

**Justification:** Ubuntu Server perfectly balances stability, security features, and learning value while meeting all coursework requirements (AppArmor, easy UFW, unattended upgrades).

## Workstation Configuration Decision
I chose **Option A – Full Ubuntu Desktop VM** on the same host.

**Justification:**
* Provides native SSH client and GUI tools (gnome-screenshot, draw.io, OBS)
* Mirrors a real-world administrator workstation
* Allows easy screenshot capture and video recording
* Demonstrates full remote administration capability as required by the brief

## Network Configuration Documentation
**VirtualBox network:** Host-Only Adapter (`vboxnet0`)
* **Network:** `192.168.56.0/24`
* **DHCP:** Disabled (static IPs used)

| Device | IP Address | Role |
| :--- | :--- | :--- |
| **Server VM** | `192.168.56.10` | Ubuntu Server (headless) |
| **Workstation VM** | `192.168.56.11` | Ubuntu Desktop (admin) |

**Configuration Steps in VirtualBox:**
1. File → Host Network Manager → Create `vboxnet0`
2. Disabled DHCP server
3. Set both VMs to use "Host-only Adapter" → `vboxnet0`

## System Specifications Using CLI Commands
All commands were run directly on the server console immediately after installation to verify the system's baseline state.

![System Specifications Screenshot](<img width="1286" height="610" alt="week1 system spec png" src="https://github.com/user-attachments/assets/272ba62f-df22-4bd8-9cef-349064dc6f09" />

*Figure 2: Terminal output showing system specifications via CLI.*)

**1. Kernel Version**
```bash
uname -a
Linux os-server 6.8.0-51-generic #51-Ubuntu SMP PREEMPT_DYNAMIC ... x86_64 GNU/Linux
````

*Confirms a 64-bit Ubuntu kernel suitable for modern security features.*

**2. Distribution Release**

```bash
lsb_release -a
Distributor ID: Ubuntu
Description:    Ubuntu 24.04.1 LTS
Release:        24.04
Codename:       noble
```

**3. Memory Usage**

```bash
free -h
              total        used        free      shared  buff/cache   available
Mem:           1.9Gi       245Mi       1.4Gi       1.0Mi       297Mi       1.6Gi
Swap:             0B          0B          0B
```

*Indicates 2GB total RAM allocated with no swap usage, minimizing I/O overhead.*

**4. Disk Usage**

```bash
df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        20G   1.8G   18G   10% /
```

*Shows a 20GB root partition with 18GB available, sufficient for future applications.*

**5. Network Interface**

```bash
ip addr show enp0s3
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> ...
    inet 192.168.56.10/24 brd 192.168.56.255 scope global enp0s3
```

*Confirms the host-only interface is active with the correct static IP.*

## Summary and Reflection

Week 1 successfully established the foundation for the entire coursework. I now have:

  * A clear, professional architecture diagram
  * A fully justified choice of Ubuntu Server 24.04 LTS
  * A properly configured isolated network
  * Baseline system specifications for future comparison

**Reflection:**
This planning phase was crucial. Choosing Ubuntu Server gives me access to modern security tools (AppArmor, UFW, unattended-upgrades) while remaining beginner-friendly. The host-only network perfectly satisfies the "no external access" requirement while allowing full SSH functionality. I learned how VirtualBox networking works in depth and feel confident moving into Week 2's security planning.

```
```
