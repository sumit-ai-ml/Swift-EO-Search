# Swift EO Search

Training-free embedding compression for remote sensing retrieval. Benchmarks TurboQuant against 8 other quantization methods across 6 foundation models and 2 datasets.

**Main finding:** TurboQuant's retrieval recall depends on the coordinate independence of the embedding distribution (Pearson r = -0.951). Contrastive and self-distillation models compress well. MAE models don't.

## Results

R@10 / Kendall's τ at 4 bits per dimension across 6 foundation models and 2 datasets. Each cell is R@10 / τ; **Tr.** marks methods that require training data; **TQ MSE** is our headline training-free method.

```latex
\begin{table}[t]
\centering
\scriptsize
\caption{EuroSAT (16K vectors). R@10\,/\,Kendall's $\tau$ at 4 bits per dimension (1 bit for RaBitQ and Binary Hash).}
\label{tab:eurosat}
\setlength{\tabcolsep}{3.5pt}
\begin{tabular}{lccccccl}
\hline
\textbf{Method} & \textbf{DINOv2} & \textbf{RemoteCLIP} & \textbf{GeoRSCLIP} & \textbf{SSL4EO} & \textbf{MAE-base} & \textbf{Prithvi} & \textbf{Tr.} \\
\hline
PQ            & \textbf{.960\,/\,.975} & \textbf{.961\,/\,.970} & \textbf{.965\,/\,.970} & \textbf{.968\,/\,.983} & \textbf{.953\,/\,.979} & \textbf{.961\,/\,.987} & yes \\
TQ Adaptive   & .942\,/\,.960 & .912\,/\,.940 & .880\,/\,.913 & .842\,/\,.913 & .863\,/\,.968 & .782\,/\,.926 & yes \\
\hline
\textbf{TQ MSE} & \textbf{.943\,/\,.959} & \textbf{.911\,/\,.938} & \textbf{.882\,/\,.910} & \textbf{.834\,/\,.906} & \textbf{.859\,/\,.966} & \textbf{.779\,/\,.923} & \textbf{no} \\
SimHash Multi & .792\,/\,.824 & .751\,/\,.779 & .708\,/\,.745 & .743\,/\,.803 & .744\,/\,.909 & .702\,/\,.867 & no \\
Uniform SQ    & .660\,/\,.719 & .549\,/\,.623 & .499\,/\,.575 & .544\,/\,.649 & .558\,/\,.829 & .502\,/\,.761 & no \\
RaBitQ        & .659\,/\,.722 & .567\,/\,.654 & .501\,/\,.582 & .544\,/\,.657 & .558\,/\,.833 & .502\,/\,.770 & no \\
Binary Hash   & .654\,/\,.705 & .607\,/\,.661 & .576\,/\,.626 & .609\,/\,.704 & .179\,/\,.227 & .451\,/\,.601 & no \\
FlyHash       & .592\,/\,.653 & .545\,/\,.605 & .497\,/\,.568 & .468\,/\,.599 & .209\,/\,.274 & .468\,/\,.725 & no \\
RandProj      & .724\,/\,.749 & .718\,/\,.744 & .671\,/\,.718 & .518\,/\,.718 & .562\,/\,.832 & .394\,/\,.729 & no \\
\hline
\end{tabular}
\end{table}
```

```latex
\begin{table}[t]
\centering
\scriptsize
\caption{BigEarthNet (269K vectors). R@10\,/\,Kendall's $\tau$ at 4 bits per dimension (1 bit for RaBitQ and Binary Hash).}
\label{tab:bigearthnet}
\setlength{\tabcolsep}{3.5pt}
\begin{tabular}{lccccccl}
\hline
\textbf{Method} & \textbf{DINOv2} & \textbf{RemoteCLIP} & \textbf{GeoRSCLIP} & \textbf{SSL4EO} & \textbf{MAE-base} & \textbf{Prithvi} & \textbf{Tr.} \\
\hline
PQ            & \textbf{.947\,/\,.945} & \textbf{.944\,/\,.936} & \textbf{.950\,/\,.944} & \textbf{.955\,/\,.959} & \textbf{.935\,/\,.930} & \textbf{.925\,/\,.943} & yes \\
TQ Adaptive   & .898\,/\,.884 & .887\,/\,.865 & .834\,/\,.814 & .777\,/\,.804 & .744\,/\,.751 & .584\,/\,.651 & yes \\
\hline
\textbf{TQ MSE} & \textbf{.900\,/\,.884} & \textbf{.878\,/\,.860} & \textbf{.830\,/\,.811} & \textbf{.770\,/\,.795} & \textbf{.737\,/\,.744} & \textbf{.572\,/\,.641} & \textbf{no} \\
SimHash Multi & .688\,/\,.641 & .648\,/\,.595 & .601\,/\,.573 & .651\,/\,.650 & .573\,/\,.582 & .481\,/\,.575 & no \\
Uniform SQ    & .479\,/\,.477 & .399\,/\,.401 & .349\,/\,.385 & .422\,/\,.468 & .338\,/\,.402 & .255\,/\,.399 & no \\
RaBitQ        & .479\,/\,.482 & .418\,/\,.429 & .348\,/\,.391 & .422\,/\,.476 & .337\,/\,.410 & .256\,/\,.412 & no \\
Binary Hash   & .483\,/\,.481 & .473\,/\,.461 & .447\,/\,.457 & .468\,/\,.503 & .128\,/\,.253 & .273\,/\,.381 & no \\
FlyHash       & .432\,/\,.440 & .409\,/\,.419 & .340\,/\,.384 & .354\,/\,.433 & .152\,/\,.275 & .207\,/\,.382 & no \\
RandProj      & .595\,/\,.562 & .619\,/\,.568 & .505\,/\,.522 & .336\,/\,.468 & .251\,/\,.348 & .073\,/\,.250 & no \\
\hline
\end{tabular}
\end{table}
```


## Models

**MAE / Reconstruction:**
- **Prithvi-EO-1.0-100M** (Jakubik et al., 2023): ViT-MAE on Sentinel-2, d=768
- **MAE-base** (He et al., 2022): ViT-MAE on ImageNet, d=768
- **SSL4EO** (Wang et al., 2022): ViT-B/16 MAE on Sentinel-1/2, d=768

**Contrastive:**
- **RemoteCLIP ViT-B-32** (Liu et al., 2024): CLIP on RS image-text pairs, d=512
- **GeoRSCLIP ViT-L-14**: CLIP ViT-L-14 (OpenAI pretrained), d=768

**Self-distillation:**
- **DINOv2 ViT-B** (Oquab et al., 2024): Self-distillation, d=768

## Datasets

- **EuroSAT**: 16,200 Sentinel-2 patches, 10 land-use classes, 64x64 pixels
- **BigEarthNet-S2**: 269,695 Sentinel-2 patches, 43 multi-label classes, 120x120 pixels

## Setup

```bash
python -m venv .venv && source .venv/bin/activate
bash setup.sh
```

Requires Python 3.10+, PyTorch 2.0+ with CUDA. Tested on NVIDIA RTX A3000 (6GB).

## Reproducing Results

### Quick start (EuroSAT, 2 original models)
```bash
bash run.sh
```

### Full reproduction

**Phase 0 — Sanity check** (no GPU needed):
```bash
python sanity_check.py
```

**Phase 1 — Extract embeddings:**
```bash
# Original 2 models (Prithvi + RemoteCLIP)
python extract.py --model prithvi --dataset eurosat
python extract.py --model remoteclip --dataset eurosat
python extract.py --model prithvi --dataset bigearthnet
python extract.py --model remoteclip --dataset bigearthnet

# Additional 4 models (DINOv2, GeoRSCLIP, MAE-base, SSL4EO)
python extract_additional.py --model all          # EuroSAT
python extract_additional_ben.py --model all       # BigEarthNet (~2hr/model)
```


**Phase 3 — Benchmark:**
```bash
# Full 9-method sweep (original 2 models)
python benchmark.py --model all --dataset eurosat --method all
python benchmark.py --model all --dataset bigearthnet --method all

# 6-model isotropy test (TQ + PQ + BinHash only)
# Results generated by inline scripts — see generate_paper_assets_v2.py
```

**Phase 4 — Generate paper assets:**
```bash
python generate_paper_assets_v2.py
```

Generates 6 figures (PNG+PDF), LaTeX tables, and CSV exports in `figures/` and `results/`.

### Run individual methods
```bash
python benchmark.py --method turboquant_mse --bits 2 3 4
python benchmark.py --method product_quant --bits 4 --seeds 42
python benchmark.py --method rabitq
```

## Key Findings

1. **Coordinate independence predicts TQ recall** with r = -0.951 across 6 models. This is a stronger predictor than KS D statistic (r = -0.507) or training paradigm labels.

2. **TurboQuant MSE is the best training-free method** across all 6 models and both datasets, 9-23% ahead of SimHash (runner-up).

3. **The Beta codebook is essential.** Uniform quantization with the same rotation gives 2.2x worse recall. But a data-adaptive codebook gives only +1% over the fixed Beta codebook.

4. **DINOv2 (self-distillation) compresses best**, even better than CLIP models. Self-distillation produces the most isotropic embeddings.

5. **The model choice matters as much as the quantizer choice.** Switching from Prithvi to DINOv2 improves TQ R@10 from 0.572 to 0.900 on BigEarthNet. That's a bigger gain than switching from TQ to PQ.

6. **QJL correction hurts.** TurboQuant's "Prod" variant degrades recall for cosine retrieval.

7. **Uniform SQ is insensitive to bits** at high d. At d=768, rotated coordinates live in +/-0.036. A [-1,1] grid wastes 96% of bins.

## Project Structure

```
quantize.py                 # All 9 quantization methods
benchmark.py                # Phase 3: recall benchmarks
extract.py                  # Phase 1: Prithvi + RemoteCLIP extraction
extract_additional.py       # Phase 1: DINOv2, GeoRSCLIP, MAE-base, SSL4EO (EuroSAT)
extract_additional_ben.py   # Phase 1: same 4 models on BigEarthNet
validate.py                 # Phase 2: Beta distribution validation
analyze.py                  # Phase 4: summary tables and plots
generate_paper_assets_v2.py # Phase 4: all paper figures, LaTeX, CSV
sanity_check.py             # Phase 0: synthetic data validation
config.py                   # Experiment configuration
utils.py                    # Rotation matrices, normalization, metrics
setup.sh                    # Install dependencies
run.sh                      # Quick pipeline (EuroSAT only)
paper.md                    # Full paper draft
```

## Outputs

```
figures/
  fig1_correlation_vs_recall.{png,pdf}  # Coord corr vs TQ R@10 (the money plot)
  fig2_recall_vs_bits.{png,pdf}         # R@10 vs bits per model
  fig3_training_free_methods.{png,pdf}  # All training-free methods compared
  fig4_codebook_ablation.{png,pdf}      # Beta vs Adaptive vs Uniform
  fig5_six_model_bars.{png,pdf}         # 6-model grouped bar chart
  fig6_scaling.{png,pdf}                # EuroSAT vs BigEarthNet scaling
  qq_*.png                              # QQ plots for Beta validation

results/
  six_model_results.json                # EuroSAT 6-model results
  six_model_ben_results.json            # BigEarthNet 6-model results
  benchmark_results.json                # Full 9-method benchmark (all seeds)
  rabitq_results.json                   # RaBitQ results
  table_6model.tex                      # LaTeX tables for paper
  paper_results.csv                     # CSV export for custom analysis
```

