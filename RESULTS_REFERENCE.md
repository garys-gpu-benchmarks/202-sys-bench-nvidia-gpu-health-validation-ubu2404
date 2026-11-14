# Results: 202 NVIDIA System Validation (clocks, temperature, etc.)

**Main goal:** Analyze GPU CUDA runtime environment health  
**Tools:** Run nvidia-smi, nvidia-smi -q, nvidia-smi -L, and dmesg

Reference results measured by the maintainer on the system shown in each section (generator TEMPLATE_00_140 · recorded 2026-11-13). Your own runs write `results/RESULTS_ACTUAL.md` in the same format.

> Smoke runs use deliberately small sizes. They check that the workload installs and runs end to end; their numbers are not performance results. Baseline and extended are.

<!-- profile:smoke -->
## smoke

- **System:** 1× NVIDIA H100 PCIe (81559 MiB) · Intel(R) Xeon(R) Platinum 8352Y CPU @ 2.20GHz · Ubuntu 24.04.5 LTS · kernel 6.8.0-138-generic · driver 595.91.07
- **Run:** 2026-11-13T20:08:19Z · status **N/A-EXPECTED** (exit 0)
- **Time (mm:ss):** setup 00:54 · benchmark 00:07 · whole run 01:09
- **Source:** run 202_20261113_200819
- **Command:** `bash run_benchmark.sh --smoke --monitor-duration-sec 5 --expected-gpu-count 1 --min-pcie-link-width 16`

| # | Metric | Result |
|---|---|---|
| 1 | GPU fault/Xid count (dmesg_nvidia_gpu_fault_xid_count) | na (expected: dmesg not readable) |
| 2 | GPU temperature (gpu_temperature_c) | 30.0 |
| 3 | PCIe link width, lanes (pcie_link_width_lanes) | 16.0 |
| 4 | PCIe link speed, GT/s (pcie_link_speed_gt_s) | 16.0 |
| 5 | CUDA devices visible (cuda_devices_visible) | 1.0 |
<!-- /profile:smoke -->
