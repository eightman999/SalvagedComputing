# Architecture

## 1. Scope

SalvagedComputing の対象は **Nukui Extreme Node（貫井の異種GPU牧場）** を中心とする salvage-oriented compute substrate です。

`llm_master_now` の RTX 3060 + Tesla P100 構成とは分けて扱います。`llm_master_now` は比較的まとまったローカルLLM実験機、SalvagedComputing は **大量の異種・旧世代・低帯域デバイスに役割を与える実験基盤** です。

## 2. Reference host

```text
BIOSTAR TB250-BTC PRO
├─ PCIe 3.0 x16 ×1
├─ PCIe x1 ×11
└─ Intel Celeron G3930
```

この構成では「全GPUを1つの巨大モデルへ束ねる」ことを既定にしません。

理由:

- x1リンクが多く、頻繁なGPU間・CPU-GPU転送を前提にした同期並列は不利
- 世代・ベンダ・VRAM・対応runtimeが揃わない
- Celeron G3930はCPU-heavy preprocessingを担わせるには弱い
- 古いカードは新しいカードと同じCUDA/toolchainで安定するとは限らない

したがって、原則は **GPUごとの独立role + 非同期ジョブ** です。

## 3. Role-oriented topology

```text
                 scheduler / control plane
                          │
           ┌──────────────┼──────────────┐
           │              │              │
       training       inference      odd-compute
         lane            lane            lane
           │              │              │
       P100 etc.       P40 etc.     old / 4GB GPUs
           │              │              │
           └──── artifacts / metrics ────┘
                          │
                    storage / YLSB
```

### Training lane

高帯域、FP16学習適性、十分な単体VRAMを優先する。

現在の基準候補は Tesla P100 16GB。

### Inference lane

VRAM容量と量子化推論適性を優先する。

現在の候補は Tesla P40 24GB。

### Large-setting / slow lane

速度より **単一GPUに収まること** を優先する探索用lane。

Tesla M40 24GB は、十分安価なら J128/J160 のような「容量が先に効く」探索のslow node候補とする。高速training nodeの代替とはみなさない。

### Odd-compute lane

4GB級、古いコンシューマGPU、ベンダ混在GPUは、標準Transformerを無理に走らせるのではなく、以下のような低メモリ・構造特化workloadを探索する。

- SSM / recurrent architecture
- sparse / linear / reduced attention
- DLGN / LUT / Boolean network
- embedding / lightweight classifier
- preprocessing / media / display
- isolated service worker

役割が見つからなければ `DISPLAY_ONLY` / `UNSUPPORTED` / `UNUSABLE` と記録する。

## 4. PCIe policy

### x16 slot

優先度:

1. host-device bandwidthが重要なaccelerator
2. 24GB級のslow training / large-setting accelerator
3. topology比較のreference card

M40 24GBのようなlarge-VRAM cardを本格的に学習へ使う場合は、原則としてx16直結候補とする。

### x1 slots

x1 riserは、通信を頻繁に行うtensor-parallel用途には期待しない。

適する候補:

- modelを一度ロードした後の独立推論
- 長時間の独立job
- GPU-local compute
- low-duty service
- architecture / runtime compatibility testing

帯域制約そのものを測定対象として保存する。

## 5. CPU / preprocessing policy

Nukui host の G3930 に重いtokenizer training、dataset preprocessing、圧縮展開、compileなどを集中させない。

可能なら別ノードで事前処理し、Nukuiには実行に必要なmaterialized artifactを渡す。

```text
fast CPU node
  -> tokenize / preprocess / build
  -> immutable artifact
Nukui node
  -> accelerator experiment
```

これによりGPUの価値と弱いhost CPUの影響を分離して評価できる。

## 6. Software/runtime lanes

古いTesla世代と新しいconsumer GPUを、無理に1つの最新toolchainへ統一しない。

最低でも以下を論理的に分ける。

- **legacy NVIDIA lane**: P100 / P40 / M40等を、その世代で安定するCUDA/runtimeで維持
- **modern NVIDIA lane**: RTX 3060等の新しいtoolchainを許可
- **AMD / alternate lane**: ROCm/Vulkan/other backendsを個別評価

コンテナ、venv、別rootfs、別hostのいずれで分離してもよいが、結果にはruntime/driver/backend versionを必ず残す。

## 7. Multi-GPU policy

異種GPUを束ねる場合は「VRAMが足せるから」だけで採用しない。

必ず比較する:

- single GPU
- layer split / pipeline-like split
- independent parallel jobs

GPU間のP2P非対応、PCIe x1、世代差がある場合は、通信量が少ないlayer placementを優先する。頻繁なall-reduceやtensor-parallelは原則として非既定。

## 8. Cooling / power

旧Tesla等のpassive server cardは、購入価格だけで評価しない。

必要記録:

- board power / measured wall power
- idle / loaded temperature
- fan / duct configuration
- auxiliary power cable / connector requirement
- PSU rail / connector availability
- throttlingの有無

passive acceleratorは**強制風冷前提**で扱い、机上のopen-air運用を標準構成にしない。

## 9. Artifact contract

各実験は最低限以下を保存する。

```text
run/
├─ manifest.json
├─ hardware.json
├─ topology.json
├─ result.json
├─ execution.log
└─ notes.md
```

`hardware.json` には少なくとも GPU model / VRAM / bus width / negotiated PCIe link / driver / runtime を含める。

`result.json` のfailure class候補:

- `completed`
- `oom`
- `timeout`
- `unsupported_runtime`
- `driver_failure`
- `thermal_limit`
- `power_limit`
- `pcie_bottleneck`
- `unusable`

## 10. Relationship with YLSB

SalvagedComputingは**探索と実験台**、YLSBは**標準試験**。

```text
SalvagedComputing exploration
        ↓
configuration looks meaningful
        ↓
repeat / stabilize
        ↓
YLSB candidate
        ↓
standardized comparable result
```

SalvagedComputing固有の面白さや安価さを、YLSBのscoreへ暗黙に混ぜない。
