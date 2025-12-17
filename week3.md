# Week 3: Application Selection for Performance Testing

## Introduction
This week focuses on selecting applications for performance evaluation, representing various workload types to test the Ubuntu Server's behavior under different conditions. This aligns with learning outcomes by synthesizing knowledge of hardware constraints and performance considerations. All installations will be SSH-based from the workstation.

## Application Selection Matrix
The matrix lists selected applications for each workload type, with justifications based on their ability to simulate real-world loads.

| Workload Type | Application | Justification |
|---------------|-------------|---------------|
| **CPU-Intensive** | `stress` | Simulates multi-core CPU load; widely used for synthetic benchmarks. |
| **RAM-Intensive** | `memtester` | Tests memory allocation and errors under pressure; ideal for detecting RAM bottlenecks. |
| **I/O-Intensive** | `fio` | Flexible disk I/O benchmarking; quantifies throughput for storage-bound tasks. |
| **Network-Intensive** | `iperf3` | Measures bandwidth and latency between server/client. |
| **Server Application** | Minecraft Server | Represents mixed loads (CPU, RAM, I/O, Network); popular game server for real-world testing. |

## Installation Documentation
All installations are performed via SSH from the workstation (`ssh admin@192.168.56.10`).

**Commands:**
1. **Update:** `sudo apt update && sudo apt upgrade -y`
2. **Tools:** `sudo apt install stress memtester fio iperf3 -y`
3. **Minecraft:**
   * Install Java: `sudo apt install openjdk-21-jre-headless -y`
   * Download: `wget https://piston-data.mojang.com/.../server.jar -O minecraft_server.jar`
   * Run: `java -Xmx1024M -Xms1024M -jar minecraft_server.jar nogui`

![Place your Installation Screenshot here](![week3-install](https://github.com/user-attachments/assets/82a07637-b745-4f30-bec1-15ad247726c4)


## Expected Resource Profiles
Based on benchmarks, anticipated usage for each app under load (on 2GB RAM, 2-core VM):

| Application | Expected CPU | RAM (MB) | Disk I/O | Network |
|-------------|--------------|----------|----------|---------|
| **stress** | 90-100% | 100-200 | Low | None |
| **memtester** | 10-20% | 1500+ | Low | None |
| **fio** | 20-40% | 200 | 100+ MB/s | None |
| **iperf3** | 10-30% | 100 | Low | 500+ Mbps |
| **Minecraft** | 50-80% | 800-1200 | 10-50 MB/s | 100-300 Mbps |

## Monitoring Strategy
For each app, measure via remote SSH:
* **Baseline:** Idle for 5 mins
* **Load:** 10 min run
* **Post-Optimization:** Retest

**Tools:** `htop` (CPU/RAM), `iostat` (I/O), `iperf3 -c` (network), `ping` (latency).

## Summary and Reflections
Week 3 selects apps for diverse workloads, with installation/monitoring plans. This prepares for testing, demonstrating critical selection.

**Reflection:**
Choices balance simplicity and realism, with trade-offs like fio's flexibility vs. dd's ease. Builds skills in benchmarking, addressing sustainability via efficient tests.
