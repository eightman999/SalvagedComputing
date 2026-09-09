# AMD Radeon RX 5500 XT

> SalvagedComputing reference — consumer RDNA1 salvage. Access date: 2026-09-10.

## Identity
- **Product**: AMD Radeon RX 5500 XT
- **GPU**: Navi 14 XTX
- **Architecture**: RDNA (1st gen), 7 nm
- **Compute units / SP**: 22 CU / **1408** stream processors
- **Clocks (AMD press)**: Game ~1717 MHz; Boost up to **1845** MHz; up to ~**5.2 TFLOPS**

## Memory
- **VRAM**: **4 GB or 8 GB** GDDR6
- **Bus**: 128-bit
- **Bandwidth**: ~**224 GB/s**

## Power
- **Typical board power (TBP)**: ~**130 W**

## ROCm / compute status
- **ROCm: NOT officially supported** for Navi consumer cards (ROCm GitHub issue #1306).
- Community gfx1012 experiments exist — mark **UNCERTAIN / unsupported for production**.
- Prefer **Vulkan** (e.g. llama.cpp) over ROCm.

## AI/ML relevance
Low-cost RDNA1 node for **small** local inference via Vulkan. Prefer **8GB** SKU. Do not plan production HIP/ROCm stacks on this card.

## Sources
- https://www.amd.com/en/newsroom/press-releases/2019-12-12-amd-unveils-the-amd-radeon-rx-5500-xt-graphics-ca.html
- https://github.com/ROCm/ROCm/issues/1306
