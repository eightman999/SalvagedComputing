# Hardware Registry

This document records **role hypotheses**, not purchase endorsements. A card becomes part of SalvagedComputing only after price, power, cooling, runtime support, and a useful workload are checked.

## Classification axes

Every device should eventually be recorded on separate axes rather than with one vague “good/bad” label.

| Axis | What to record |
|---|---|
| VRAM capacity | usable single-device memory |
| memory bandwidth | measured/spec reference |
| compute | useful precision paths, not only headline FLOPS |
| PCIe | physical slot + negotiated width/generation |
| runtime support | driver/CUDA/ROCm/Vulkan/runtime ceiling |
| training utility | what can actually train |
| inference utility | quant/runtime/model combinations |
| power | idle/load board or wall power |
| thermals | cooling requirement + steady-state temperature |
| acquisition cost | dated observed price, shipping excluded/included noted |
| operator cost | setup pain, custom cooling, adapters, unstable software |

## Current / known pool

### Nukui Extreme Node host

| Component | Status | Notes |
|---|---|---|
| BIOSTAR TB250-BTC PRO | owned / reference | x16 ×1 + x1 ×11 topology |
| Intel Celeron G3930 | owned / reference | keep control-plane work light; preprocess elsewhere |
| RX 5500 XT | pool candidate | AMD alternate-runtime lane |
| RX 6400 | pool candidate | constrained card; measure before assigning role |
| GTX 550 Ti | pool candidate | extreme legacy classification target |
| GT 710 / 730 | pool candidate | likely lightweight/display/runtime-bound roles; preserve failure evidence |

## NVIDIA candidate matrix

### Tesla P100 16GB

**Role:** main old-GPU training reference.

Why it matters:

- already proved useful for real training workloads
- enough VRAM to remain experimentally relevant
- serves as a much stronger “old accelerator” baseline than treating every legacy GPU as equivalent

Policy:

- use for actual training when it fits
- retain as comparison point when testing cheaper/slower 24GB cards
- do not waste it on workloads that can run on much weaker devices

### Tesla P40 24GB

**Role hypothesis:** large-VRAM quantized inference / model residency node.

Current decision:

- preferred over M40 when price is close enough because the target is not merely “own 24GB” but obtain a useful 24GB lane
- benchmark actual runtime support and quantized inference rather than assuming theoretical INT8 capability guarantees modern software efficiency

Dated user-reported market snapshot, 2026-09-09:

- Tesla P40 24GB: **about ¥39,000** candidate listing discussed

This price is historical context only; verify listing, condition, cooling and shipping before purchase.

### Tesla M40 24GB

**Role hypothesis:** slow, cheap, large-setting node.

Current decision:

- only compelling when **24GB is genuinely cheap**
- not a P100 replacement for training throughput
- value comes from fitting larger single-device experiments where wall-clock time is secondary
- if used for training, prefer the direct x16 slot and forced-air cooling

Good candidate workloads:

- J128/J160-like capacity-bound experiments
- compatibility / fit tests
- large model residency where newer instruction paths are not mandatory

Bad reason to buy:

- “24GB automatically means good LLM GPU”

### Quadro RTX 4000 / Quadro M4000 class

A roughly **¥47,000** candidate listing was discussed on 2026-09-09. The exact product/condition of a slash-labeled listing must be verified before treating the number as comparable market data.

Current position:

- do not buy merely because it is workstation-branded
- compare usable VRAM, generation, runtime support and price against P40/P100-class alternatives

### RTX 3060 12GB

**Role:** modern consumer reference, not the SalvagedComputing target identity.

Use it to answer:

> Is the salvaged configuration useful relative to a readily understood modern baseline?

Do not move the whole project toward ordinary RTX benchmarking; YLSB / llm_master already cover the mainstream comparison side better.

## Extreme legacy classification

Some devices may never be useful for modern LLM inference. That is still a result.

Suggested terminal classifications:

- `GENERAL_USEFUL` — broadly useful for at least one current AI workload
- `NICHE_USEFUL` — useful only with a specific architecture/runtime/role
- `EXPERIMENT_ONLY` — educational / research value but poor operational value
- `DISPLAY_ONLY` — practical use is graphics/display only
- `UNSUPPORTED` — software stack prevents meaningful use
- `THERMALLY_IMPRACTICAL` — cooling cost/complexity defeats the role
- `POWER_IMPRACTICAL` — energy cost defeats the role
- `UNUSABLE` — no defensible role found

A device may move between classifications as runtimes or architectures change. Keep date and software version with every classification.

## Purchase rule

Before buying another accelerator, answer all five:

1. **What experiment or workload becomes possible?**
2. **Why can an owned device not already do it?**
3. **Which slot / power / cooling path will it use?**
4. **Which runtime lane supports it?**
5. **What measurement decides whether the purchase was worthwhile?**

If those cannot be answered, do not buy it for SalvagedComputing merely because the listing is cheap.

## Priority as of 2026-09-09

1. Make the existing Nukui node measurable and reproducible.
2. Preserve P100 as the old-GPU training reference.
3. Consider P40 24GB as the stronger practical 24GB candidate.
4. Consider M40 24GB only at a sufficiently low salvage price for capacity-bound work.
5. Explore tiny/old GPUs with architectures that suit them rather than forcing dense Transformer inference.
