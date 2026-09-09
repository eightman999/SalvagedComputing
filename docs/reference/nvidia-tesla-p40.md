# NVIDIA Tesla P40 24GB

> SalvagedComputing reference — large-VRAM quantized / INT8-era Pascal. Access date: 2026-09-10.

## Identity
- **Product**: NVIDIA Tesla P40
- **Architecture**: Pascal **GP102** (model often cited GP102-895-A1)
- **Compute capability**: **6.1** (`sm_61`)
- **CUDA cores**: 3840
- **Clocks (product literature)**: base ~1303 MHz / boost ~1531 MHz

## Memory
- **VRAM**: **24 GB GDDR5**
- **Bandwidth**: up to **347 GB/s**
- **Memory bus**: 384-bit

## Power / form factor
- **Total board power**: **250 W**
- **Cooling**: Passive bidirectional airflow
- **Aux power**: CPU 8-pin
- **Interface**: PCIe 3.0 x16, dual-slot (~10.5")
- **ECC**: configurable; default on

## Performance notes
- **INT8**: ~**47 TOPS** (NVIDIA P40 literature / INT8 announcement era)
- **FP32**: ~**12 TFLOPS** (datasheet)

## Driver / CUDA ceiling
- Pascal CC 6.1: CUDA offline compile through **12.x**; dropped in **13.0**; driver ceiling **R580**

## AI/ML relevance (SalvagedComputing)
Preferred **24GB** salvage lane vs M40 when price is close — better for quantized inference / INT8-era workloads. Lower memory bandwidth than P100 HBM2.

## Sources
- https://images.nvidia.com/content/pdf/tesla/Tesla-P40-Product-Brief.pdf
- https://images.nvidia.com/content/pdf/tesla/184427-Tesla-P40-Datasheet-NV-Final-Letter-Web.pdf
- https://developer.nvidia.com/cuda/gpus/legacy
- https://docs.nvidia.com/datacenter/tesla/drivers/cuda-toolkit-driver-and-architecture-matrix.html
