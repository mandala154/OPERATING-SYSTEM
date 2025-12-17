# Week 2: Security Planning and Testing Methodology

## Introduction
This week focuses on designing a security baseline and performance testing methodology for the Ubuntu Server setup planned in Week 1. All planning is documented here to demonstrate progressive learning.

The methodology emphasizes remote CLI-based approaches via SSH, aligning with the coursework's administrative constraints and employability skills in system hardening and monitoring.

## Performance Testing Plan
This plan describes a remote monitoring methodology and testing approach for evaluating the server's performance under different workloads. The goal is to measure OS behavior quantitatively, identifying bottlenecks and optimizations while adhering to sustainability principles.

### Remote Monitoring Methodology
* **Tools:** Utilize CLI tools accessible via SSH for low-overhead monitoring. Primary tools include `htop` for interactive metrics, `iostat` for disk I/O, `iperf3` for network bandwidth, and `ping` for latency.
* **Execution:** From the workstation, connect via SSH and run tools directly on the server, redirecting outputs to logs.
* **Rationale:** This remote method ensures no graphical tools are used on the server, reducing energy consumption and simulating a real-world headless environment.

### Testing Approach
I will test the following scenarios for each selected application:
1.  **Baseline Testing:** Measure idle state for 5 minutes to establish norms.
2.  **Application Load Testing:** Run the application at varying intensities (e.g., 100% load) for 10 minutes.
3.  **Performance Analysis:** Identify bottlenecks (e.g., high CPU utilization >80%) using tools like `top` and `iostat`.
4.  **Optimization Testing:** Implement improvements (e.g., process priority changes), retest, and compare results.

**Metrics to Collect:**
* CPU Usage (%)
* Memory Usage (Used/Available)
* Disk I/O (Reads/Writes per sec)
* Network Throughput (Mbps)
* System Latency (ms)

## Security Configuration Checklist
This checklist outlines the planned configurations for hardening the Ubuntu Server. Implementation will begin in Week 4.

| Category | Configuration Steps | Justification |
| :--- | :--- | :--- |
| **SSH Hardening** | 1. Generate ED25519 keys.<br>2. Disable password authentication.<br>3. Disable root login.<br>4. Limit max auth tries. | Prevents brute-force attacks; ED25519 offers superior security and performance over RSA. |
| **Firewall (UFW)** | 1. Install UFW.<br>2. Deny incoming by default.<br>3. Allow SSH only from Workstation IP.<br>4. Enable logging. | Reduces attack surface by restricting access to trusted IPs only. |
| **Access Control** | 1. Enforce AppArmor profiles.<br>2. Monitor logs for denials. | Confines processes to prevent privilege escalation. |
| **Auto Updates** | 1. Configure `unattended-upgrades`.<br>2. Enable security updates.<br>3. Disable auto-reboot to prevent downtime. | Ensures timely patching against vulnerabilities without manual intervention. |
| **User Management** | 1. Create non-root admin user.<br>2. Configure `sudo` with timeouts.<br>3. Lock root password. | Enforces least privilege principle and prevents direct root access. |
| **Network Security** | 1. Disable unused services (IPv6 if not used).<br>2. Scan ports locally. | Minimizes potential entry points for attackers. |

## Threat Model
This model identifies specific security threats to the Linux server using the STRIDE framework, with planned mitigations.

| Threat (STRIDE) | Description | Mitigation Strategy | Risk Level |
| :--- | :--- | :--- | :--- |
| **Brute-Force Attacks (Spoofing)** | Automated credential guessing on SSH, leading to unauthorized access. | Implement key-based authentication, disable passwords, and use Fail2Ban. | **High** |
| **Privilege Escalation (Elevation)** | Exploiting kernel flaws or misconfigurations to gain root access. | Use non-root users, restrict `sudo` access, and enforce AppArmor profiles. | **Medium** |
| **Software Vulnerabilities (Info Disclosure)** | Outdated packages enabling exploits like ransomware or denial of service. | Enable automatic security updates and minimize running services. | **High** |

## Summary and Reflection
Week 2 established the planning for security and performance, building on Week 1's architecture. The performance plan uses remote CLI tools for efficient testing, while the checklist and threat model form a robust hardening strategy. This planning phase revealed the interconnections between security configurations and performance metrics, preparing the ground for the implementation phase.
