# NVIDIA GeForce GT 710 / GT 730 (extreme legacy)

> SalvagedComputing reference — teaching dead-ends / negative controls. Access date: 2026-09-10.
>
> **CRITICAL:** GT 730 shipped as multiple SKUs with different architectures. Always identify with `nvidia-smi` / `deviceQuery` before assuming compute capability.

## GT 730 variants (archived GeForce specs)

| Variant | CUDA cores | Clock | VRAM | Bandwidth | Board power (approx) | Typical CC |
|---|---:|---:|---|---:|---:|---|
| DDR3 128-bit | 96 | ~700 MHz | 1024 MB DDR3 | 28.8 GB/s | ~49 W | often **Fermi 2.1** |
| DDR3 64-bit | (verify SKU) | 902 MHz | 2048 MB DDR3 | 14.4 GB/s | ~23 W | Kepler-class *(verify)* |
| GDDR5 64-bit | 384 | 902 MHz | 1024 MB GDDR5 | 40 GB/s | ~25 W | many listed **Kepler 3.5** |

## GT 710 (typical)
- Often **GF119 Fermi**, compute capability **2.1**
- Common: ~1 GB DDR3, **64-bit** bus
- TDP roughly **19–29 W**
- Bandwidth often ~**14 GB/s** *(secondary — uncertain)*

## Driver / CUDA ceilings
- **Fermi CC 2.1**: CUDA **8.0** / **R390**
- **Kepler CC 3.5**: CUDA **11.x** / **R470**

## AI/ML relevance
Not practical for LLM work. Preserve as negative-control evidence.

## Sources
- https://web.archive.org/web/20151212232644/http://www.geforce.com/hardware/desktop-gpus/geforce-gt-730/specifications
- https://developer.nvidia.com/cuda/gpus/legacy
- https://docs.nvidia.com/datacenter/tesla/drivers/cuda-toolkit-driver-and-architecture-matrix.html
