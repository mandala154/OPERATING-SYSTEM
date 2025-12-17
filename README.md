# OPERATING-SYSTEM
# Operating Systems Coursework (CMPN202)

**Student Name:** Akshay Kumar Mandal
**Student ID:** A00024985
**Module:** Operating Systems (CMPN202)
**Institution:** University of Roehampton

## Project Overview
This repository documents the practical implementation of a dual-VM Linux architecture for the Operating Systems coursework. Over 7 weeks, I designed, deployed, secured, and analyzed a headless Ubuntu Server managed entirely via SSH from a separate Workstation VM.

The project demonstrates proficiency in:
* **System Architecture:** Designing isolated networks (Host-Only) for secure testing.
* **Server Hardening:** Implementing SSH keys, UFW firewalls, AppArmor, and Fail2Ban.
* **Performance Analysis:** Benchmarking CPU, RAM, and Disk I/O using CLI tools.
* **Automation:** Writing Bash scripts for security baselining and remote monitoring.

## System Architecture
The setup consists of two Virtual Machines running on VirtualBox:

| Role | OS Distribution | IP Address | Configuration |
| :--- | :--- | :--- | :--- |
| **Server** | Ubuntu Server 24.04 LTS | `192.168.56.10` | Headless, SSH only, Hardened |
| **Workstation** | Ubuntu Desktop 24.04 LTS | `192.168.56.11` | GUI Admin Console, Monitoring Station |

* **Network:** VirtualBox Host-Only Adapter (`vboxnet0`) for complete isolation.

## Weekly Journal
Click the links below to view the detailed documentation for each phase.

| Week | Phase | Key Activities |
| :--- | :--- | :--- |
| **[Week 1](week1.md)** | **System Planning** | Architecture design, distro selection, and network setup. |
| **[Week 2](week2.md)** | **Security Planning** | Threat modeling, security checklist, and testing plan. |
| **[Week 3](week3.md)** | **App Selection** | Choosing and installing workloads (`stress`, `fio`, `Minecraft`). |
| **[Week 4](week4.md)** | **Initial Config** | SSH hardening (Keys), Firewall (UFW) setup, and User management. |
| **[Week 5](week5.md)** | **Advanced Security** | AppArmor, Fail2Ban, Auto-updates, and Bash scripting. |
| **[Week 6](week6.md)** | **Performance** | Load testing, bottleneck analysis, and system optimization. |
| **[Week 7](week7.md)** | **Security Audit** | Lynis auditing, Nmap scanning, and final evaluation. |

## Tools & Technologies Used
* **Virtualization:** Oracle VirtualBox
* **Operating Systems:** Ubuntu Linux 24.04 LTS
* **Security:** OpenSSH, UFW, AppArmor, Fail2Ban, Lynis, Nmap
* **Performance:** Htop, Iostat, Iperf3, Stress, Memtester, Fio
* **Scripting:** Bash (Shell Scripting), Python (Data Visualization)
