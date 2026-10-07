# iPerf3 TCP/UDP Sweep Benchmark

[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)
[![CI](https://img.shields.io/badge/CI-host--safe-green.svg)](.github/workflows/ci.yml)

Target: Ubuntu 26.04 · AMD · see Hardware Requirements. This is a host benchmark, not a laptop `pip install` project.

## Quick Start

```bash
git clone https://github.com/garys-gpu-benchmarks/308-sys-bench-amd-iperf3-network-performance-ubu2604.git
cd 308-sys-bench-amd-iperf3-network-performance-ubu2604
sudo bash setup.sh --assume-yes
bash run_benchmark.sh --profile smoke --validate
```
Results are written to `results/benchmark.db` and `results/summary.json`.

This workload is executed on the validation host after the repository is copied there. `setup.sh` and `run_benchmark.sh` do not open an outbound SSH session.

Prerequisites: Ubuntu 26.04; AMD; Python 3.14.4; root or sudo for `setup.sh`. Framework: Bash, SQLite, Python, PyYAML, iPerf3. This is a host benchmark, not a laptop `pip install` project.

```mermaid
flowchart LR
  setup.sh --> run_benchmark.sh --> parse_results.py --> results/benchmark.db
```

## 1. Overview

Runs iperf3 JSON client tests against a one-shot loopback server on 127.0.0.1. protocol: tcp,udp. port: listen/connect port with +1/+2 fallback. time: seconds. Sweep dimensions: protocol, port, reverse_mode, window_size, buffer_length, parallel_streams, bandwidth_limit, omit_initial_seconds.

## 2. What It Validates

- Validates iperf3 TCP and UDP JSON runs over loopback (127.0.0.1) on one host. This is not a NIC or VM-to-VM test
- #1: TCP loopback throughput (tcp_loopback_gbps); is present and physically sensible.
- #2: UDP loopback throughput (udp_throughput_gbps); is present and physically sensible.
- #3: UDP jitter (udp_jitter_ms); is present and physically sensible.
- #4: Packet loss, percent (packet_loss_rate); is present and physically sensible.
- #5: TCP retransmit count per run (tcp_retransmit_count) is present and physically sensible.

## 3. Metrics Captured

- **#1: TCP loopback throughput** — stored as `tcp_loopback_gbps`.
- **#2: UDP loopback throughput** — stored as `udp_throughput_gbps`.
- **#3: UDP jitter** — stored as `udp_jitter_ms`.
- **#4: Packet loss, percent** — stored as `packet_loss_rate`.
- **#5: TCP retransmit count per run** — stored as `tcp_retransmit_count`.

## 4. Hardware Requirements

### Supported environment

- OS: Ubuntu 26.04
- GPU vendor: AMD
- Framework family: Bash, SQLite, Python, PyYAML, iPerf3
- Python: Python 3.14.4

### Reference validation environment

The tables below describe the machine used to generate the reference results. They are not a requirement that every user buy that exact cloud instance.

### System

Ubuntu 26.04 / AMD / Bash, SQLite, Python, PyYAML, iPerf3

### GPU

Ubuntu 26.04 / AMD / Bash, SQLite, Python, PyYAML, iPerf3

## 5. Software Requirements

| Component | Version |
|---|---|
| OS | Ubuntu 26.04 |
| Kernel | kernel 7.0.0 |
| Python | Python 3.14.4 |
| ROCm | N/A - ROCm not used |
| rocBLAS | N/A - rocBLAS not used |

Runs iperf3 JSON client tests against a one-shot loopback server on 127.0.0.1. protocol: tcp,udp. port: listen/connect port with +1/+2 fallback.

## 6. Installation

```bash
Run iperf3 -s -1 then iperf3 -c 127.0.0.1 -J for each yaml protocol
```

## 7. Running the Benchmark

```bash
Run iperf3 -s -1 then iperf3 -c 127.0.0.1 -J for each yaml protocol
```

**Validating results separately:**

```bash
export BENCHMARK_PYTHON=/usr/bin/python3.13  # optional
python3 -m venv .venv
source ".venv/bin/activate"
".venv/bin/python" scripts/validate_results.py
```

## 8. Output

### `results/benchmark.db` (SQLite)

iperf3 -J JSON plus one normalized CSV row per protocol

sample_index,status,protocol,parallel_streams,tcp_loopback_gbps,udp_throughput_gbps,udp_jitter_ms,packet_loss_rate,tcp_retransmit_count,bits_per_second,error_message
0,ok,tcp,1,40,,,,,40000000000,

```bash
Run iperf3 -s -1 then iperf3 -c 127.0.0.1 -J for each yaml protocol
```

### `results/summary.json`

Consolidated metrics from the most recent run — suitable for CI artifact upload or dashboard ingestion.

### `results/raw/<timestamp>.txt`

iperf3 -J JSON plus one normalized CSV row per protocol

sample_index,status,protocol,parallel_streams,tcp_loopback_gbps,udp_throughput_gbps,udp_jitter_ms,packet_loss_rate,tcp_retransmit_count,bits_per_second,error_message
0,ok,tcp,1,40,,,,,40000000000,

## 9. Baselines / Thresholds

Expected ranges and gates live in `config/benchmark_config.yaml` under `baselines:` or `thresholds:`. To update them, edit that file — never edit validation code directly.

## 10. Troubleshooting

**`setup.sh` missing collector**
Create cannot finish without `scripts/collect_workload.py`.

**`self_check` overlay rewritten**
Do not overwrite files listed in `results/overlay_lock.json`.

**Remote SSH drop during setup**
Reconnect and resume `bash setup.sh --assume-yes`. Do not wipe `.venv` or `.cache`.

## 11. NVIDIA H100 Coding Differences

Primary target is AMD ROCm. NVIDIA notes in this section are reference only and are not the execution path.

## Repository layout

```text
.
├── setup.sh
├── run_benchmark.sh
├── benchmark_specification.json
├── config/
├── scripts/
├── src/
├── tests/
├── docs/
├── results/
└── LICENSE
```
