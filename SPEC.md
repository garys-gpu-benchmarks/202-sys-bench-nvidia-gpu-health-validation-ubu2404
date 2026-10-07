# SPEC.md: "The Human How"; Exact technical requirements, environment setup, implementation details, etc.

## Execution Description

Polls NVIDIA GPU health twice (~1 s apart) from nvidia-smi and dmesg. Yaml flags pcie_check, clock_check, temperature_check, power_check, ecc_check, retired_pages_check, firmware_version_check, cuda_device_check, cuda_context_check, and dmesg_check are documented but ignored by the collector. Always records device count, GPU temperature, PCIe width/speed, and case-insensitive dmesg 'xid' matches. output_format: csv Sweep dimensions: pcie_check, clock_check, temperature_check, power_check, ecc_check, retired_pages_check, firmware_version_check, cuda_device_check.

## Parameters

| Parameter | CLI Flag | Tested Values | Default | Description |
| --- | --- | --- | --- | --- |
| pcie_check | `--pcie-check` | smoke=true, baseline=true, extended=true | true | From Parameter list; see Execution Description With Parameters. |
| clock_check | `--clock-check` | smoke=true, baseline=true, extended=true | true | From Parameter list; see Execution Description With Parameters. |
| temperature_check | `--temperature-check` | smoke=true, baseline=true, extended=true | true | From Parameter list; see Execution Description With Parameters. |
| power_check | `--power-check` | smoke=true, baseline=true, extended=true | true | From Parameter list; see Execution Description With Parameters. |
| ecc_check | `--ecc-check` | smoke=true, baseline=true, extended=true | true | From Parameter list; see Execution Description With Parameters. |
| retired_pages_check | `--retired-pages-check` | smoke=true, baseline=true, extended=true | true | From Parameter list; see Execution Description With Parameters. |
| firmware_version_check | `--firmware-version-check` | smoke=true, baseline=true, extended=true | true | From Parameter list; see Execution Description With Parameters. |
| cuda_device_check | `--cuda-device-check` | smoke=true, baseline=true, extended=true | true | From Parameter list; see Execution Description With Parameters. |
| cuda_context_check | `--cuda-context-check` | smoke=true, baseline=true, extended=true | true | From Parameter list; see Execution Description With Parameters. |
| dmesg_check | `--dmesg-check` | smoke=true, baseline=true, extended=true | true | From Parameter list; see Execution Description With Parameters. |
| output_format | `--output-format` | smoke=csv, baseline=csv, extended=csv | csv | From Parameter list; see Execution Description With Parameters. |

## Invocation

```bash
Run nvidia-smi, nvidia-smi -q, nvidia-smi -L, and dmesg
```

## Raw Output Format

CSV aggregate plus the dmesg transcript. A single row is duplicated to two samples

sample_index,status,dmesg_nvidia_gpu_fault_xid_count,gpu_temperature_c,cuda_devices_visible,pcie_link_width_lanes,pcie_link_speed_gt_s,error_message
0,ok,0,42,1,16,32,

## Metrics

- **#1: GPU fault/Xid count** — stored as `dmesg_nvidia_gpu_fault_xid_count`.
- **#2: GPU temperature** — stored as `gpu_temperature_c`.
- **#3: PCIe link width, lanes** — stored as `pcie_link_width_lanes`.
- **#4: PCIe link speed, GT/s** — stored as `pcie_link_speed_gt_s`.
- **#5: CUDA devices visible** — stored as `cuda_devices_visible`.

## Framework

Polls NVIDIA GPU health twice (~1 s apart) from nvidia-smi and dmesg. Yaml flags pcie_check, clock_check, temperature_check, power_check, ecc_check, retired_pages_check, firmware_version_check, cuda_device_check, cuda_context_check, and dmesg_check are documented but ignored by the collector. Always records device count, GPU temperature, PCIe width/speed, and case-insensitive dmesg 'xid' matches.

## Installation and Execution Summary

Run nvidia-smi, nvidia-smi -q, nvidia-smi -L, and dmesg twice about one second apart, then parse GPU temperature, PCIe width/speed, visible device count, and case-insensitive dmesg Xid matches, to measure GPU health. CLI --pcie-check/--ecc-check and similar flags are ignored

## Platform Portability

- **AMD (primary):** ```bash
Run nvidia-smi, nvidia-smi -q, nvidia-smi -L, and dmesg
```
- **NVIDIA:** Native NVIDIA CUDA workload. Execute on the stated Ubuntu release with the host NVIDIA driver and CUDA userspace. ROCm porting notes do not apply.

## Model Context Protocols

- **Active:** None

## Execution-Loop Validation Contract

EXECUTION CHAIN: `run_benchmark.sh` ➔ raw output ➔ `scripts/parse_results.py` ➔ `results/benchmark.db` ➔ `scripts/validate_results.py`

This benchmark uses a lightweight, SQLite-integrated execution loop for result validation. All validation is performed by `scripts/validate_results.py`.

### Validation script usage

```bash
export BENCHMARK_PYTHON=/usr/bin/python3.13  # optional; select the installed interpreter

# After a live run:
".venv/bin/python" scripts/validate_results.py --db results/benchmark.db

# CI / no-GPU path (seeds fixture and validates it):
".venv/bin/python" scripts/validate_results.py --seed-fixture --quiet

# Override DB path via environment variable:
BENCHMARK_DB=tests/fixtures/benchmark.db \
  ".venv/bin/python" scripts/validate_results.py
```

### Run artifact contract

CSV aggregate plus the dmesg transcript. A single row is duplicated to two samples

sample_index,status,dmesg_nvidia_gpu_fault_xid_count,gpu_temperature_c,cuda_devices_visible,pcie_link_width_lanes,pcie_link_speed_gt_s,error_message
0,ok,0,42,1,16,32,

```bash
bash run_benchmark.sh --help
bash run_benchmark.sh --profile smoke --validate
bash run_benchmark.sh --profile baseline --validate
bash run_benchmark.sh --profile extended --validate
```
`run_benchmark.sh --help` prints usage and exits. The harness calls `scripts/ensure_setup.sh` when `.setup_state` is absent.

### Required integrity checks (built into `validate_results.py`)

1. Latest run exists and `runs.status = 'ok'`.
2. `run.error_message` is NULL.
3. `started_at` and `finished_at` are valid ISO-8601 UTC strings.
4. All required aggregate metrics in `runs` are non-NULL and finite.
5. All required aggregate metrics are physically sensible (positive values). Polls NVIDIA GPU health twice (~1 s apart) from nvidia-smi and dmesg. Yaml flags pcie_check, clock_check, temperature_check, power_check, ecc_check, retired_pages_check, firmware_version_check, cuda_device_check, cuda_context_check, and dmesg_check are documented but ignored by the collector. Always records device count, GPU temperature, PCIe width/speed, and case-insensitive dmesg 'xid' matches.
6. At least 2 sample rows exist for the latest `run_id` (sweep coverage).
7. No sample has `status = 'error'`.
8. Polls NVIDIA GPU health twice (~1 s apart) from nvidia-smi and dmesg. Yaml flags pcie_check, clock_check, temperature_check, power_check, ecc_check, retired_pages_check, firmware_version_check, cuda_device_check, cuda_context_check, and dmesg_check are documented but ignored by the collector. Always records device count, GPU temperature, PCIe width/speed, and case-insensitive dmesg 'xid' matches.

### Baseline / Threshold configuration (`config/benchmark_config.yaml`)

Expected ranges and gates live in `config/benchmark_config.yaml` under `baselines:` or `thresholds:`. To update them, edit that file — never edit validation code directly.

Threshold key suffixes encode comparison direction when `thresholds:` is present: `_min` → observed value must be ≥ threshold. `_max` → observed value must be ≤ threshold. Informational `baselines:` ranges are not pass/fail gates.
