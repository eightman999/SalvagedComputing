# NVIDIA Tesla P100 16GB

> SalvagedComputing reference — old-GPU training baseline. Access date: 2026-09-10.

## Identity
- **Product**: NVIDIA Tesla P100 PCIe 16GB (PB-08248-001_v01)
- **Architecture**: Pascal **GP100**, TSMC 16nm FinFET
- **Compute capability**: **6.0** (`sm_60`)
- **CUDA cores**: 3584
- **Clocks (product brief)**: base **1189** MHz / boost **1328** MHz

## Memory
- **VRAM**: **16 GB HBM2**
- **Bandwidth**: up to **732 GB/s**
- **Memory clock**: 715 MHz (HBM2)
- **Memory bus**: 4096-bit HBM2

## Power / form factor
- **Total board power**: **250 W**
- **Cooling**: Passive bidirectional heatsink (needs chassis airflow)
- **Aux power**: **CPU 8-pin** connector (often needs CPU↔PCIe dongle)
- **Interface**: PCIe 3.0 x16, dual-slot (~10.5" Form Factor 3.0)
- **ECC**: enabled by default

## Driver / CUDA ceiling
- Pascal CC 6.0: last CUDA toolkit offline compile in **CUDA 12.x**; removed in **CUDA 13.0**
- Last driver branch for Maxwell/Pascal/Volta: **R580**

## AI/ML relevance (SalvagedComputing)
Primary **old training reference**. Strong FP16 path for Pascal-era training; excellent memory bandwidth vs GDDR5 Tesla cards. Prefer for real training when it fits; do not waste on workloads weaker cards can handle.

## Sources
- https://www.nvidia.com/content/dam/en-zz/Solutions/Data-Center/tesla-product-literature/NV-tesla-p100-pcie-PB-08248-001-v01.pdf
- https://images.nvidia.com/content/pdf/tesla/whitepaper/pascal-architecture-whitepaper-v1.2.pdf
- https://developer.nvidia.com/cuda/gpus/legacy
- https://docs.nvidia.com/datacenter/tesla/drivers/cuda-toolkit-driver-and-architecture-matrix.html
