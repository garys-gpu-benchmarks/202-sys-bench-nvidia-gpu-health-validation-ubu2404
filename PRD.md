# PRD.md:  "The Why"; Product requirements, benchmark metadata table, high-level requirements, etc.

Product Requirements Document

"The Why"; Product requirements, benchmark metadata table, high-level requirements, etc. Defines the benchmark goal, validation objective, test name, benchmark number, category, and high-level success criteria.

## Benchmark Matrix Document Metadata (via benchmark_specification.json)

This PRD.md section is populated from benchmark_specification.json, which is the structured source of benchmark-specific product requirements.

## Workload Number
202

## Workload Name
NVIDIA System Validation (clocks, temperature, etc.)

## Execution Summary (Run and Measure)
Run nvidia-smi, nvidia-smi -q, nvidia-smi -L, and dmesg twice about one second apart, then parse GPU temperature, PCIe width/speed, visible device count, and case-insensitive dmesg Xid matches, to measure GPU health. CLI --pcie-check/--ecc-check and similar flags are ignored

## Main Goal
Analyze GPU CUDA runtime environment health

## Validation Objective
Validates GPU visibility, temperature, PCIe width/speed, and dmesg Xid mentions. Does not run DCGM, deviceQuery, or CUDA context creation

## Workload Category
System Validation & Reliability

## Validation Requirement

The benchmark must include an automated SQLite-integrated validation layer that verifies persisted results from `results/benchmark.db`. Validation must confirm:

1. The benchmark run completed successfully with no tool errors.
2. Required samples and aggregate metrics were persisted for every swept shape.
3. Metrics are finite and physically sensible (positive, within plausible bounds).
4. Measured values satisfy configured thresholds when the workload defines pass/fail gates.
5. The benchmark fails validation when required data is missing, invalid, or outside bounds.

## Non-Functional Requirements

| Requirement | Target |
|---|---|
| Automation | Runs to completion without manual intervention after `bash run_benchmark.sh` |
| Idempotency | Re-running `run_benchmark.sh` appends a new run; never corrupts existing rows |
| Persistence | All metrics survive script exit; `results/benchmark.db` is the durable record |
