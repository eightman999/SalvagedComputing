# Roadmap

## P0 — Make Nukui measurable

Before buying more GPUs, make the existing node produce reproducible evidence.

### P0.1 Inventory snapshot

Create a script that records:

- motherboard / CPU / RAM
- `lspci -nn`
- GPU identity and PCI IDs
- driver versions
- negotiated PCIe generation / width
- VRAM
- temperature / fan / power telemetry where available
- block devices / filesystem free space
- OS / kernel

Output: `inventory/<date>/hardware.json` + raw command logs.

### P0.2 Runtime matrix

For every accelerator, record whether each lane is:

- installs
- detects device
- loads model/kernel
- executes
- stable for 30+ min

Initial runtime candidates:

- llama.cpp / CUDA
- PyTorch CUDA
- Vulkan where useful
- ROCm / AMD-compatible paths where applicable

Do not mark a GPU unsupported because only one runtime failed.

### P0.3 Thermal / power baseline

For each passive/server GPU:

- define forced-air setup
- idle 10 min
- sustained load 30 min
- record temperature, clocks, throttle status and wall power if measurable

Abort conditions must be explicit before unattended runs.

## P1 — Build the role classifier

Create a small reproducible battery that answers:

1. Can it initialize a modern runtime?
2. How much VRAM is actually usable?
3. Can it run FP32 / FP16 / INT8 / low-bit paths relevant to the backend?
4. Is the workload compute-bound, memory-bound, PCIe-bound, runtime-bound or thermally limited?
5. What is the cheapest useful role?

Output one provisional label:

- `GENERAL_USEFUL`
- `NICHE_USEFUL`
- `EXPERIMENT_ONLY`
- `DISPLAY_ONLY`
- `UNSUPPORTED`
- `THERMALLY_IMPRACTICAL`
- `POWER_IMPRACTICAL`
- `UNUSABLE`

## P2 — Validate known role hypotheses

### Tesla P100 16GB

Goal: establish an old-GPU training reference.

Tests:

- single-GPU training throughput
- context / batch fit boundary
- hardware-aware shape sweep
- compare against RTX 3060 where scientific comparison makes sense

### Tesla P40 24GB

Only after acquisition.

Goal: test whether 24GB + quantized inference produces better practical value than older/slower 24GB alternatives.

Tests:

- GGUF / supported quant inference
- model fit boundary
- prompt vs generation throughput
- power and cooling
- x16 vs constrained PCIe where possible

### Tesla M40 24GB

Only if acquisition cost justifies a slow 24GB lane.

Goal: answer whether cheap single-card capacity is useful despite poor wall-clock performance.

Tests:

- maximum practical model / experiment fit
- slow training viability
- x16 dependency
- thermal / power overhead
- direct comparison with P100 for equal workload and with P40 for equal VRAM class

## P3 — Extreme legacy lane

Do not begin with LLM chat benchmarks.

Candidate experiment families:

- DLGN / recurrent DLGN
- LUT / Boolean networks
- tiny recurrent LM
- SSM-style small model
- sparse / local attention
- embeddings / classifier
- media preprocessing
- isolated inference microservice

The success criterion is not “beats RTX 3060.”

Success means **a defensible useful role per yen / watt / owned hardware exists**.

## P4 — Remote experiment executor

Once manual runs stabilize, implement a constrained executor.

Input contract:

```json
{
  "experiment": "...",
  "device": "...",
  "runtime": "...",
  "command_profile": "...",
  "wallclock_limit_s": 0,
  "disk_limit_bytes": 0
}
```

Output contract:

```text
run_summary.json
hardware.json
topology.json
artifacts.json
execution.log
```

Security policy:

- no arbitrary credential access
- no arbitrary home-network traversal
- workspace-scoped writes
- explicit approved command profiles
- revocable remote access

## P5 — YLSB promotion path

A configuration can be proposed to YLSB only after:

- hardware identity is stable
- runtime is pinned
- command/config is reproducible
- repeated runs are consistent enough to interpret
- failure modes are classified

SalvagedComputing can remain messy. YLSB should not.

## Backlog

- [ ] inventory snapshot tool
- [ ] PCIe topology visualizer
- [ ] runtime compatibility matrix
- [ ] thermal / power test protocol
- [ ] hardware role classifier
- [ ] P100 shape sweep derived from hardware co-design ideas
- [ ] x16 vs x1 model-load / inference comparison
- [ ] low-bit inference backend comparison
- [ ] DLGN / LUT experimental lane
- [ ] remote executor artifact contract
- [ ] purchase ledger with dated price + shipping + cooling/adapters
- [ ] generate YLSB candidate manifests from stable SalvagedComputing runs

## Non-goals for now

- buying every cheap accelerator
- pretending heterogeneous x1 GPUs form one fast monolithic GPU
- hiding failed experiments
- optimizing leaderboard score before measurement infrastructure exists
- turning SalvagedComputing into Kamimusuhi or novllm itself
