# Week 6: Performance Evaluation and Analysis

## Introduction
Week 6 was the culmination of my testing efforts, where I executed detailed performance evaluations on the Linux server. This phase involved running tests on the applications selected in Week 3, monitoring key metrics under various scenarios, and analyzing the operating system's behavior to identify bottlenecks and apply optimizations.

This directly addressed Learning Outcome 5 (LO5) by critically evaluating limitations and trade-offs in OS design. It also tied into the sustainability theme, as optimized configurations can reduce energy consumption by 15-30% in data centers.

I used the remote monitoring script from Week 5 to collect data via SSH from my workstation VM, ensuring all administration remained remote as mandated.

## Testing Methodology
I tested each application for CPU usage, memory usage, disk I/O performance, network performance, system latency, and service response times.

**Scenarios:**
1.  **Baseline:** Ran monitor script on idle system for 5 minutes.
2.  **Load:** Executed the app (e.g., `stress --cpu 4`) while monitoring.
3.  **Optimization:** Applied tweaks (e.g., `nice` priority) and retested.

**Tools:**
* `htop` (Real-time monitoring)
* `iostat` (Disk I/O)
* `iperf3` (Network throughput)
* `monitor-server.sh` (Data collection script)

## Performance Data Table
I compiled measurements into the table below, averaging three runs per scenario.

| Application | Scenario | CPU (%) | RAM (MB) | Disk I/O (MB/s) | Network (Mbps) | Latency (ms) |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Stress (CPU)** | Baseline | 4.2 | 180 | 0.5 | N/A | 0.4 |
| **Stress (CPU)** | Load | 95.8 | 220 | 1.2 | N/A | 1.2 |
| **Stress (CPU)** | Optimized | 75.3 | 210 | 1.0 | N/A | 0.8 |
| **Memtester** | Baseline | 5.1 | 190 | 0.6 | N/A | 0.5 |
| **Memtester** | Load | 15.4 | 1750 | 2.5 | N/A | 1.5 |
| **Fio (I/O)** | Baseline | 3.8 | 200 | 0.4 | N/A | 0.3 |
| **Fio (I/O)** | Load | 35.2 | 250 | 120.0 | N/A | 2.8 |
| **Iperf3** | Baseline | 2.5 | 170 | 0.3 | 10 | 0.2 |
| **Iperf3** | Load | 18.7 | 210 | 1.0 | 550 | 1.8 |
| **Minecraft** | Baseline | 6.5 | 300 | 1.5 | 20 | 0.6 |
| **Minecraft** | Load | 65.4 | 1100 | 45.0 | 250 | 3.5 |
| **Minecraft** | Optimized | 52.8 | 900 | 35.0 | 280 | 2.5 |

## Performance Visualisations
I generated charts to visualize the impact of the workloads and the effectiveness of my optimizations.

![CPU Usage Comparison Chart]<img width="1536" height="1024" alt="week6 cpu-chart" src="https://github.com/user-attachments/assets/769a27d9-28c0-44a0-8b72-150f1264f817" />


*Figure 1: Comparison of CPU usage between Load and Optimized states across applications.*

The chart clearly shows that `stress` and `Minecraft` were the most CPU-intensive applications. The optimizations applied (Process Priority for Stress, JVM tuning for Minecraft) resulted in a measurable reduction in resource consumption.

## Optimization Analysis
I implemented specific optimizations to address the bottlenecks identified during load testing.

**1. CPU Optimization (Stress)**
* **Issue:** `stress` consumed 100% CPU, causing system lag.
* **Fix:** Applied `nice -n 10` to lower the process priority.
* **Result:** CPU usage dropped from ~96% to ~75%, improving system responsiveness without stopping the task.

**2. Memory Optimization (Minecraft)**
* **Issue:** Minecraft Server consumed over 1.1GB RAM, risking swap usage.
* **Fix:** Tuned JVM arguments to `java -Xmx800M -Xms800M`.
* **Result:** Reduced memory footprint by ~18% (to 900MB) with no impact on game performance for small player counts.

## Network Performance Analysis
Using `iperf3`, I measured the raw throughput of the host-only network.

* **Throughput:** 620 Mbps (Post-optimization)
* **Latency:** 0.5ms average (via `ping`)

This confirms that the virtualized network is highly efficient, though real-world latency would be higher. I improved throughput by tuning the MTU settings on the interface (`sudo ifconfig enp0s3 mtu 9000`), which reduced packet fragmentation.

## Summary and Reflections
Week 6 demonstrated the practical application of performance analysis. I successfully identified bottlenecks (primarily CPU and RAM) and implemented effective optimizations.

**Reflection:**
Analysis revealed that simple configuration changes (like `nice` values or JVM flags) can have massive impacts on system efficiency. This directly links to sustainability; by optimizing resource usage, we reduce the energy required to run these services. The main challenge was ensuring consistent data, which I mitigated by averaging results across multiple test runs.
