# SalvagedComputing — GPU Reference Index

> Practical AI on obsolete / cheap / unusual GPUs（限界カスGPU鯖計画）.
> Access date: **2026-09-10**.

## Reference node (host)
- **Board**: BIOSTAR **TB250-BTC PRO**
- **CPU**: Intel Celeron **G3930**
- **PCIe**: 3.0 **x16×1** + **x1×11**

### Topology caveats
- Many **x1** slots are **bandwidth-starved** for multi-GPU training/inference.
- Passive Tesla boards (**P100 / P40 / M40**) need strong **chassis airflow**.
- Tesla boards often use a **CPU 8-pin** auxiliary connector — may need a **CPU 8-pin ↔ PCIe 8-pin** dongle.

## GPU reference pages

| Page | Role in SalvagedComputing |
| --- | --- |
| [nvidia-tesla-p100.md](./nvidia-tesla-p100.md) | Old training reference (CC **6.0**, 16GB HBM2) |
| [nvidia-tesla-p40.md](./nvidia-tesla-p40.md) | Quantized / INT8-era inference, **24GB** GDDR5 |
| [nvidia-tesla-m40.md](./nvidia-tesla-m40.md) | Cheap slow **24GB** Maxwell |
| [amd-rx-5500-xt.md](./amd-rx-5500-xt.md) | RDNA1 consumer; **ROCm unsupported** |
| [amd-rx-6400.md](./amd-rx-6400.md) | Low-power RDNA2; **4GB**; ROCm unsupported |
| [nvidia-gtx-550-ti.md](./nvidia-gtx-550-ti.md) | Fermi extreme legacy |
| [nvidia-gt-710-730.md](./nvidia-gt-710-730.md) | Fermi/Kepler SKU maze |
| [sources.md](./sources.md) | Bibliography (URLs + access dates) |

## Notes
- These pages are **reference** docs (datasheet summaries, CC/CUDA ceilings, gotchas). They intentionally do **not** duplicate the project README/architecture registry.
- RTX 3060 12GB is a modern comparison target only — light notes deferred / out of this batch unless added later.
