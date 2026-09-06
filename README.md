# ids-privacy-transformations

Code and experiments for **"Balancing Privacy and Detection Utility in
Flow-Based Intrusion Detection: A Cross-Dataset Study of Progressive
Feature Transformations."**

This repository implements and evaluates a set of progressive
privacy-preserving feature transformations (S0–S4) for a flow-based
intrusion detection system (IDS), under both **frozen-model** and
**retrained-model** deployment protocols. Experiments are conducted on
CICIDS2017, with cross-dataset validation on CIC-IDS-2018.

## Repository structure

```
ids-privacy-transformations/
├── notebooks/
│   ├── ids_complete_experiments.ipynb        # Main experiments: S0–S4, E5 (temporal
│   │                                          # generalization sensitivity), E6
│   │                                          # (feature-group ablation)
│   └── ids_retraining_privacy_metrics.ipynb  # Retrained-model protocol, privacy
│                                              # metrics, and CIC-IDS-2018 cross-
│                                              # dataset validation
├── models/
│   ├── savedmodel/            # Original pre-trained (frozen) IDS classifier
│   ├── retrained_S0.keras     # Retrained models per scenario (CICIDS2017)
│   ├── retrained_S1.keras
│   ├── retrained_S2.keras
│   ├── retrained_S3.keras
│   ├── retrained_S4.keras
│   └── 2018/
│       ├── retrained_2018_S0.keras  # Retrained models per scenario (CIC-IDS-2018)
│       ├── retrained_2018_S1.keras
│       ├── retrained_2018_S2.keras
│       ├── retrained_2018_S3.keras
│       └── retrained_2018_S4.keras
├── figures/                # All figures reported in the paper
├── results/
│   ├── multiseed_results.csv  # Per-run results: 5 scenarios × 5 seeds (25 runs)
│   ├── multiseed_summary.csv  # Aggregated mean ± std per scenario
│   └── results_2018.json      # CIC-IDS-2018 cross-dataset results
├── data/
│   └── README.md           # Links and instructions to obtain train/test data
├── requirements.txt
├── SETUP.md                # Detailed setup guide and troubleshooting
├── LICENSE
└── README.md
```

## Requirements

- **Python 3.11.x** (newer versions are not supported — see `SETUP.md`)
- A CPU with **AVX/AVX2 support** (standard on any machine from 2013 onward;
  low-power CPUs such as Intel Atom/Celeron/Pentium "N"-series are not
  supported). Google Colab can be used as a free alternative if your
  hardware does not meet this requirement.

## Data availability

Preprocessed train/test data for both datasets are hosted on Zenodo due to
file size constraints. See `data/README.md` for download links and
placement instructions.

- **CICIDS2017**: https://doi.org/10.5281/zenodo.22339804
- **CIC-IDS-2018**: https://doi.org/10.5281/zenodo.22543338

Raw data sources and required citations for both datasets are listed in
`data/README.md`.

## Model attribution

The original pre-trained (frozen) IDS classifier (`models/savedmodel/`) was
provided by Marija S. Milošević, used with permission, and is based on the
architecture described in:

> M.S. Milosevic, V.M. Ciric. "Extreme minority class detection in
> imbalanced data for network intrusion." *Computers & Security*
> 2022;123:102940. https://doi.org/10.1016/j.cose.2022.102940

Model architecture: feed-forward neural network, 5 hidden layers
(90-75-60-45-30 neurons), ReLU activations, softmax output over the
traffic classes of each respective dataset.

## How to run

1. Install Python 3.11.x and the required packages:
   ```
   pip install -r requirements.txt
   ```
2. Download the datasets from Zenodo (see above) and place them in `data/`.
3. Open `notebooks/ids_complete_experiments.ipynb` in Jupyter, update the
   `PATH` and `MODEL_PATH` variables in the first cell to point to your
   local `data/` and `models/` folders, then run all cells
   (**Kernel → Restart & Run All**). Full execution takes approximately
   20–30 minutes.
4. For the retrained-model protocol, privacy metrics, and CIC-IDS-2018
   cross-dataset validation, run
   `notebooks/ids_retraining_privacy_metrics.ipynb` following the same steps.

**For detailed step-by-step instructions, including environment setup and
troubleshooting common issues, see [SETUP.md](SETUP.md).**

## Citation

If you use this code or the accompanying data in your research, please cite
our paper (citation to be added upon publication) and the original dataset
sources listed in `data/README.md`.

## License

Code in this repository is released under the MIT License (see `LICENSE`).
Data hosted on Zenodo is released under CC-BY-4.0.
