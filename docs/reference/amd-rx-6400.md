# AMD Radeon RX 6400

> SalvagedComputing reference — low-power RDNA2 entry card. Access date: 2026-09-10.

## Identity
- **Product**: AMD Radeon RX 6400
- **GPU**: Navi 24
- **Architecture**: RDNA 2, TSMC 6 nm
- **Compute units / SP**: 12 CU / **768** stream processors
- **Clocks (AMD product page)**: Game **2039** MHz; Boost up to **2321** MHz
- **Infinity Cache**: **16 MB**

## Memory
- **VRAM**: **4 GB** GDDR6
- **Bus**: **64-bit**
- **Bandwidth**: up to **128 GB/s**

## Power / bus
- **Typical board power**: **53 W**
- Often no external power connector
- **PCIe 4.0 x4** — can bottleneck on mining boards / constrained lanes

## ROCm status
- Treat as **unsupported for ROCm AI**; prefer Vulkan / non-ROCm paths.

## AI/ML relevance
Low-power secondary node: display + light Vulkan inference. **4 GB** hard-limits model size.

## Sources
- https://www.amd.com/en/products/graphics/desktops/radeon/6000-series/amd-radeon-rx-6400.html
- https://rocm.docs.amd.com/projects/install-on-windows/en/latest/reference/system-requirements.html
