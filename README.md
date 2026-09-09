# SalvagedComputing

**Practical AI on obsolete, cheap, unusual, and salvaged compute hardware.**

日本語副題: **限界カスGPU鯖計画**

SalvagedComputing は、古いGPU、用途の外れたアクセラレータ、安価な中古部品、帯域や電力に制約のある構成を捨てずに、現代のローカルAI・機械学習・実験基盤としてどこまで実用化できるかを検証するプロジェクトです。

目的は「古いGPUも速い」と主張することではありません。**どこではまだ使え、どこから使えないのかを、実測し、役割を与え、再現可能な形で残すこと**です。

## Core idea

一般的なGPUサーバのように、同型の高速GPUを束ねて巨大な1モデルを走らせることを前提にしません。

SalvagedComputing では、異種ハードをそれぞれの得意分野へ割り当てます。

```text
old / cheap / odd hardware
        ↓
measure real constraints
        ↓
assign a role that matches the hardware
        ↓
run useful workloads
        ↓
record reproducible evidence
```

典型的な役割は以下です。

- dense training
- quantized inference
- large-VRAM / slow-compute experiments
- preprocessing / tokenization
- embedding / reranking
- SSM / recurrent / linear-attention experiments
- DLGN / LUT / Boolean-network experiments
- storage / networking / orchestration
- display-only / unsupported / unusable classification

## Current reference node

### Nukui Extreme Node

- Motherboard: BIOSTAR TB250-BTC PRO
- CPU: Intel Celeron G3930
- PCIe: PCIe 3.0 x16 ×1 + x1 ×11
- Concept: heterogeneous GPU farm / experimental pasture
- Known pool candidates include RX 5500 XT, RX 6400, GTX 550 Ti, GT 710 / 730 and future salvaged accelerators.

The x16 slot is reserved for workloads that truly need host-device bandwidth or a large accelerator. x1 lanes are treated as constrained experimental links rather than pretending every GPU is connected like a normal workstation card.

## Candidate accelerator roles

| Accelerator | Current role hypothesis |
|---|---|
| Tesla P100 16GB | main old-GPU training / FP16-capable compute reference |
| Tesla P40 24GB | quantized inference / large VRAM candidate |
| Tesla M40 24GB | very cheap 24GB slow node; large-setting experiments when speed is secondary |
| RTX 3060 12GB | modern comparison / fast consumer reference; not the identity of this project |
| 4GB-class / older cards | architecture-hardware co-design experiments; classify usefulness instead of forcing standard LLM workloads |

The project deliberately separates **single-GPU VRAM capacity**, **compute speed**, **memory bandwidth**, **PCIe topology**, **runtime support**, **power**, and **purchase price**. A card can be valuable even when it is slow if it unlocks an experiment that otherwise does not fit.

## Relationship to other projects

### YLSB

[YLSB](https://github.com/eightman999/YLSB) is the reproducible benchmark / standard side.

- SalvagedComputing: build, operate, salvage, assign roles, discover unusual configurations.
- YLSB: measure comparable configurations under a defined procedure.

SalvagedComputing may feed candidate configurations into YLSB, but YLSB scoring remains independent from this project's automation or narrative.

### novllm

[novllm](https://github.com/eightman999/novllm) is a model/tokenizer research workload and a useful stress test for salvaged hardware. The hardware project must not redefine novllm's scientific claims.

### Kamimusuhi

[Kamimusuhi](https://github.com/eightman999/kamimusuhi) may eventually consume heterogeneous compute as a cognitive substrate, but SalvagedComputing remains a hardware / systems project rather than a persona or agent architecture.

## Principles

1. **Measure before declaring useless.**
2. **Do not force every accelerator into the same workload.**
3. **Use hardware-aware model / runtime design.**
4. **Treat VRAM, bandwidth, compute, power, topology, software support and cost as separate axes.**
5. **Keep failures.** OOM, timeout, unsupported runtime, unusable thermals and bad price/performance are valid results.
6. **Prefer reproducible artifacts over anecdotes.**
7. **A salvaged component earns a role; it does not get a role merely because it exists.**

## Repository layout

- `docs/ARCHITECTURE.md` — heterogeneous-node architecture and role assignment
- `docs/HARDWARE_REGISTRY.md` — hardware candidates and classification scheme
- `docs/reference/` — GPU datasheet / compute-capability / CUDA-ceiling references ([index](docs/reference/INDEX.md))
- `docs/RESEARCH_NOTES_2026-09-09.md` — model/hardware co-design notes distilled from today's reading
- `docs/2026-09-09.md` — project inception / today's decisions
- `ROADMAP.md` — next experiments and build steps

## Status

Project established on **2026-09-09**.

The immediate goal is not to buy every cheap GPU. It is to turn the Nukui heterogeneous node into a reproducible experimental platform, then add hardware only when it opens a useful measurement or workload lane.
