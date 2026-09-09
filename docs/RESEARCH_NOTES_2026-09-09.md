# Research Notes — 2026-09-09

今日読んだ記事・議論から、SalvagedComputing に直接効く部分だけを残す。

これは literature review の完成版ではなく、**古い・弱い・変なハードに仕事を与えるための設計メモ**。

## 1. Hardware-aware model design

Reference:

- bilzard, 「HWの制約を考慮したモデル設計によるパフォーマンス改善」
  - https://zenn.dev/bilzard/articles/co-designing-model-architecture-with-hardware
  - original paper: *The Case for Co-Designing Model Architectures with Hardware* (2024)

### Distillation

モデル設計は「パラメータ数だけ合わせれば同程度」ではない。GPU上ではGEMM形状、tile、SM数、precision path等によって実効throughputが変わる。

記事では、Transformerの計算の大部分がGEMMにあり、行列dimensionやtile配置をGPUに合わせることで同程度のモデル規模でもthroughput改善余地があると紹介されている。

### SalvagedComputing implication

**モデルをGPUへ押し込むだけでなく、GPUに合わせてモデル側を変えることを許可する。**

特に旧GPUでは、新GPU向けに成立した「普通のhidden size / head数 / batch」をそのままコピーしない。

測る候補:

- hidden dimension alignment
- head dimension
- microbatch
- sequence length
- GEMM shape
- SM occupancy
- tensor-core pathの有無

P100のようにSM数や演算器構成が現行GPUと異なるカードでは、同一parameter countでshape違いを比較する価値がある。

## 2. Quantization is not only “make it smaller”

Reference:

- bilzard, 「LLMの推論では外れ値は重要な役割を果たす」
  - https://zenn.dev/bilzard/articles/llm-int8-matrix-multiplication
  - LLM.int8() / bitsandbytes background

### Distillation

量子化はVRAM削減の道具だが、単純に全activation/weightを同じ精度へ落とせばよいわけではない。LLM.int8() 系ではoutlierを高精度側へ分離する発想が重要だった。

また、小さな行列ではquantize/dequantize overheadでINT8化が速度向上につながらない場合もある。大規模modelはmemory-boundになりやすく、より低bitのweight quantizationが有利なケースもある。

### SalvagedComputing implication

P40等を「INT8があるから速い」と仕様表だけで評価しない。

測定軸を分ける:

- fit: modelがVRAMへ入るか
- quality: quantization degradation
- throughput
- prompt processing / generation speed
- conversion overhead
- backend-specific kernel availability

旧GPUでは理論演算性能より、**そのruntimeがそのカード向けの実装をまだ持っているか**の方が支配的になる可能性がある。

## 3. Attention is not sacred

Reference:

- bilzard, 「self-attentionを代替する各種手法について」
  - https://zenn.dev/bilzard/articles/self-attention-alternatives

### Distillation

full self-attention はsequence lengthに対する計算量・メモリ負荷が大きい。過去から sparse attention、LSH / Reformer、近似、FlashAttention等の方向が試されてきた。

### SalvagedComputing implication

4GB級・帯域の弱いGPUを「Transformerがまともに動かないからゴミ」と即断しない。

別の問題設定を与える:

- short-context specialist
- sparse / local attention
- recurrent / retention-style model
- state-space style model
- task-specific small model

目的はmainstream LLMとの勝敗ではなく、**そのハード上で最も合理的な計算構造を探すこと**。

## 4. Training architecture contains removable costs

Reference:

- bilzard, 「[survey] 近年のLLMに関する提案手法について」
  - https://zenn.dev/bilzard/articles/survey-various-methods-used-in-llm-pre-training

### Useful leads

記事で紹介される例から、制約ハード向けに追試価値があるもの:

- recurrent form / retention approaches for long context
- parallel layer calculation
- headless language modeling のようなtraining-memory削減発想
- early exiting のようなconditional compute

これらを「最新版だから採用」するのではなく、**限界hardwareでボトルネックを1個ずつ外す候補**として扱う。

## 5. Logic-gate / Boolean models as an alternate lane

References:

- 手羽先, 「2026年は論理ゲート式ニューラルネットワークが爆発的に進化する」
  - https://zenn.dev/teba_eleven/articles/68955053ed75be
- かるまる, 「論理ゲートだけで言語モデルを作って Transformer を超えるまで、3度散った話」
  - https://zenn.dev/karumaru/articles/24bca710a2db62

### Distillation

DLGN等では、学習時に論理ゲートを連続緩和して勾配を通し、最終的に離散回路へ落とす方向が研究されている。記事・実験例は、論理回路系が万能だと証明するものではないが、GPUのdense floating-point GEMMとは異なる計算基盤を探索できることを示している。

かるまる氏の記事は特に、単純なDLGNやloop化が失敗し得ることも含めて、**architectureを変えながら失敗を測る**ことの価値を示す実験例として読む。

### SalvagedComputing implication

極端に古いGPUへ最新Transformerを無理に載せるだけでなく、将来的に以下をexperimental laneとして扱う。

- DLGN
- recurrent DLGN
- LUT network
- binary / low-bit network
- FPGA / logic-oriented implementation

ただし「GPUより速い」という一般化を初期前提にしない。必ず workload、quality、hardware、power を固定して比較する。

## 6. Extreme Hardware Organ hypothesis

今日の議論から出た仮説:

> 異種GPU群は、一枚の巨大な仮想GPUとして扱うより、異なる計算器官の集合として扱う方が自然ではないか。

例:

```text
P100      -> dense-learning organ
P40       -> quantized-memory organ
M40       -> slow-large-memory organ
4GB GPU   -> small specialist / odd architecture organ
CPU node  -> preprocessing organ
storage   -> artifact / model store
network   -> transport
```

この発想はKamimusuhiにも接続可能だが、SalvagedComputing側では人格や認知モデルを前提にせず、**heterogeneous compute scheduling hypothesis**として検証する。

## 7. Research discipline

今後の記事・論文を読む時は、次の問いへ落とす。

1. 何のボトルネックを減らしているか？ VRAM / bandwidth / FLOPS / latency / communication / power?
2. どのhardware featureを前提としているか？
3. 旧GPUでその前提は成立するか？
4. 成立しない場合、別architectureなら役割を作れるか？
5. YLSBへ持ち込める再現可能なtestにできるか？

「面白い技術」から「この廃GPUに何をやらせるか」まで落とせたものだけをSalvagedComputingの候補とする。
