# Gamow–Teller $A=9$ Mirror Triplet: ML/XGBoost Discrepancy Diagnostics

This repository contains the data, analysis pipeline, and LaTeX manuscript for
*"Machine-Learning and Gradient-Boosted Identification of Gamow–Teller
Theory–Experiment Discrepancies in the $A = 9$ Mirror Triplet"*, prepared for
submission to the **Brazilian Journal of Physics**.

## Overview

We construct a verified 108-row data set of per-state $B(\mathrm{GT})$ values
(6 final states × 3 transitions × 6 shell-model calculation sets) extracted
directly from NuShellX@MSU output for the $A=9$ isobaric mirror triplet
($^9$Li→$^9$Be, $^9$Be→$^9$B, $^9$C→$^9$B), and apply a balanced Random Forest
and XGBoost to:

1. Classify (state, calculation-set) combinations as theory–experiment
   *discrepant* vs *concordant* ($|\log_{10}\rho| > 0.7$), under both
   row-wise leave-one-out (LOO-CV) and group-wise leave-one-state-out
   (LOSO-CV) cross-validation.
2. Predict the **experimental** $B(\mathrm{GT})$ strength category
   (weak/moderate/strong) from theoretical features alone.
3. Identify experimentally weak states, benchmarked against an isospin-mirror
   rule and an excitation-energy threshold.
4. Interpret the XGBoost model with SHAP feature attribution.

## Repository structure

```
.
├── data/                       Raw NuShellX@MSU / KaleidaGraph .bln output files
│   ├── 9Li_9Be.bln
│   ├── 9Be_9B.bln
│   ├── 9C_9B.bln
│   ├── Sum_BGT_9Li_9Be.bln
│   ├── Sum_9Be_9B.bln
│   └── Sum_BGT_9C_9B.bln
├── code/
│   ├── build_dataset.py        Parses data/*.bln -> gt_dataset_108_clean.csv
│   ├── ml_pipeline.py           Binary discrepancy classifier (RF + XGBoost,
│   │                            LOO-CV, group-wise LOSO-CV, bootstrap CIs,
│   │                            feature importances) -> ml_results.pkl
│   ├── ml_pipeline2.py          Three-class experimental-strength task and
│   │                            weak-state identification + baselines
│   │                            -> ml_results2.pkl
│   └── make_figures.py          Generates all manuscript figures into figures/
├── figures/                     Pre-generated PNG figures used in the manuscript
├── gt_dataset_108_clean.csv      Pre-built 108-row data set
├── main.tex                      Manuscript source (LaTeX)
├── references.bib                Bibliography
├── main.pdf                       Compiled manuscript
├── requirements.txt
└── LICENSE
```

## Reproducing the analysis

From the repository root:

```bash
pip install -r requirements.txt

# 1. Rebuild the data set from the raw .bln files (optional; the CSV is
#    already included)
python3 code/build_dataset.py

# 2. Run the binary discrepancy classification (RF vs XGBoost)
python3 code/ml_pipeline.py

# 3. Run the three-class / weak-state diagnostics
python3 code/ml_pipeline2.py

# 4. Regenerate all figures
python3 code/make_figures.py
```

## Compiling the manuscript

```bash
pdflatex main.tex
bibtex main
pdflatex main.tex
pdflatex main.tex
```

Requires a standard TeX distribution (TeX Live / MiKTeX) with `amsmath`,
`amssymb`, `booktabs`, `authblk`, `hyperref`, `graphicx`, and `titlesec`.

## Data provenance

The `.bln` files are direct exports of NuShellX@MSU shell-model output
(theoretical $B(\mathrm{GT})$ and excitation energies for six calculation
sets — CKPOT, PSDMK, and PSDMWK interactions within the p and psd model
spaces, with successive truncation choices) alongside experimental values
cross-referenced against ENSDF. All theoretical $B(\mathrm{GT})$ values
incorporate the standard mass-dependent quenching factor
$q = [1 - 0.19(A/160)^{0.35}]^2 \approx 0.87$ for $A=9$.

## Citation

If you use this data set or pipeline, please cite the associated manuscript
(citation details to be added upon publication).

## License

Code and data are released under the MIT License (see `LICENSE`). The
manuscript text and figures are © the authors.
