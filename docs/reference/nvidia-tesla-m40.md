# NVIDIA Tesla M40 24GB

> SalvagedComputing reference — cheap slow 24GB Maxwell accelerator. Access date: 2026-09-10.

## Identity
- **Product**: NVIDIA Tesla M40 24GB GPU Accelerator
- **Architecture**: Maxwell (GM200)
- **Compute capability**: **5.2** (`sm_52`)
- **CUDA cores**: 3072

## Memory
- **VRAM**: 24 GB GDDR5
- **Bandwidth**: **288 GB/s** (official datasheet)
- **Memory bus**: 384-bit *(commonly cited secondary sources)*
- **Clocks (secondary)**: base ~948 MHz / boost ~1112 MHz — verify on SKU

## Power / form factor
- **Max power**: 250 W
- **Cooling**: Passive heatsink (needs chassis airflow)
- **Interface**: PCIe 3.0 x16, dual-slot full-height (~10.5" class)
- **Display outputs**: typically none

## Performance notes
- **FP32**: up to ~7 TFLOPS with GPU Boost (datasheet)
- **FP64**: ~0.2 TFLOPS
- No Tensor cores; no P40-style INT8 path; FP16 is not Pascal-class

## Driver / CUDA ceiling
- Maxwell CC 5.2: CUDA offline compile through **12.x**; removed in **CUDA 13.0**; driver ceiling **R580**

## AI/ML relevance (SalvagedComputing)
Cheap **24GB** residency / capacity-bound experiments where tokens/s are secondary. Prefer P40 when INT8/FP16 matter; prefer P100 for training throughput.

## Sources
- https://images.nvidia.com/content/tesla/pdf/78071_Tesla_M40_24GB_Print_Datasheet_LR.PDF
- https://developer.nvidia.com/cuda/gpus/legacy
- https://docs.nvidia.com/datacenter/tesla/drivers/cuda-toolkit-driver-and-architecture-matrix.html
