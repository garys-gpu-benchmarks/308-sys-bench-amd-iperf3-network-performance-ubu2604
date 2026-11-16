# Results: 308 iPerf3 TCP/UDP Sweep

**Main goal:** Measure TCP/UDP loopback throughput and stability  
**Tools:** Run iperf3 -s -1 then iperf3 -c 127.0.0.1 -J for each yaml protocol

Reference results measured by the maintainer on the system shown in each section (generator TEMPLATE_00_142 · recorded 2026-11-16). Your own runs write `results/RESULTS_ACTUAL.md` in the same format.

> Smoke runs use deliberately small sizes. They check that the workload installs and runs end to end; their numbers are not performance results. Baseline and extended are.

<!-- profile:smoke -->
## smoke

- **System:** 1× AMD Instinct MI300X VF (196288 MB) · INTEL(R) XEON(R) PLATINUM 8568Y+ · Ubuntu 26.04.1 LTS · kernel 7.0.0-38-generic · driver 7.1.9.31600000
- **Run:** 2026-11-16T11:03:49Z · status **PASS** (exit 0)
- **Time (mm:ss):** setup 00:07 · benchmark 00:06 · whole run 00:17
- **Source:** run 308_20261116_110349
- **Command:** `bash run_benchmark.sh --smoke --protocol tcp,udp --port 5201 --reverse-mode false --window-size 256K --buffer-length 128K --parallel-streams 1 --bandwidth-limit 0 --omit-initial-seconds 0 --time 2`

| # | Metric | Result |
|---|---|---|
| 1 | TCP loopback throughput (tcp_loopback_gbps) | 56.118558567557294 |
| 2 | UDP loopback throughput (udp_throughput_gbps) | 52.683988980033796 |
| 3 | UDP jitter (udp_jitter_ms) | 0.0013517066038471926 |
| 4 | Packet loss, percent (packet_loss_rate) | 0.37517323302316374 |
| 5 | TCP retransmit count per run (tcp_retransmit_count) | 0.0 |
<!-- /profile:smoke -->
