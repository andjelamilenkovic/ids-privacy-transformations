# ids-privacy-transformations

Code and experiments for **"Balancing Privacy and Detection Utility in
Flow-Based Intrusion Detection: A Cross-Dataset Study of Progressive Feature
Transformations."**

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
│   └── ids_retraining_privacy_metrics.ipynb  # Retrained-model protocol and
│                                              # privacy metric evaluation
├── models/
│   ├── savedmodel/         # Original pre-trained (frozen) IDS classifier
│   └── retrained_S0.keras  # Retrained models, one per transformation scenario
│   └── retrained_S1.keras
│   └── retrained_S2.keras
│   └── retrained_S3.keras
│   └── retrained_S4.keras
├── figures/                # All figures reported in the paper
├── results/
│   ├── multiseed_results.csv  # Per-run results: 5 scenarios × 5 seeds (25 runs)
│   └── multiseed_summary.csv  # Aggregated mean ± std per scenario (Table 5.5)
├── data/
│   └── README.md           # Link and instructions to obtain train/test data
├── requirements.txt
├── LICENSE
└── README.md
```

## Data availability

The preprocessed `trainset.parquet` and `testset.parquet` files (derived
from CICIDS2017, 77 flow-based statistical features + 15 one-hot encoded
labels) are hosted on Zenodo due to file size constraints:

**https://doi.org/10.5281/zenodo.22339804**

Download both files and place them in the `data/` folder before running the
notebooks. See `data/README.md` for details.

Raw CICIDS2017 data is publicly available from the Canadian Institute for
Cybersecurity (CIC), University of New Brunswick:
https://www.unb.ca/cic/datasets/ids-2017.html

If you use the CICIDS2017 dataset, please cite:

> Sharafaldin, I., Lashkari, A. H., & Ghorbani, A. A. (2018). "Toward
> Generating a New Intrusion Detection Dataset and Intrusion Traffic
> Characterization." *4th International Conference on Information Systems
> Security and Privacy (ICISSP)*, Portugal, January 2018.

## Model attribution

The original pre-trained (frozen) IDS classifier (`models/savedmodel/`) was
provided by Marija S. Milošević, used with permission, and is based on the
architecture described in:

> M.S. Milošević, V.M. Ćirić. "Extreme minority class detection in
> imbalanced data for network intrusion." *Computers & Security*
> 2022;123:102940. https://doi.org/10.1016/j.cose.2022.102940

Model architecture: feed-forward neural network, 5 hidden layers
(90-75-60-45-30 neurons), ReLU activations, softmax output over 15 traffic
classes.

## How to run

1. Install dependencies:
   ```
   pip install -r requirements.txt
   ```
2. Download `trainset.parquet` and `testset.parquet` from Zenodo (see above)
   and place them in `data/`.
3. Open `notebooks/ids_complete_experiments.ipynb` in Jupyter, update the
   `PATH` variable in the data-loading cell to point to your local `data/`
   folder, and run all cells (**Kernel → Restart & Run All**).
4. Full execution takes approximately **20–30 minutes**, as the model is
   run 13 times (5 main scenarios S0–S4, 3 temporal-generalization
   scenarios E5, 5 feature-group ablation scenarios E6).
5. Final results and rankings are printed at the end of the notebook.

For the retrained-model protocol and privacy metrics, run
`notebooks/ids_retraining_privacy_metrics.ipynb` following the same steps.

## Citation

If you use this code or the accompanying data in your research, please cite
our paper (citation to be added upon publication) and the original
CICIDS2017 dataset as noted above.

## License

Code in this repository is released under the MIT License (see `LICENSE`).
Data hosted on Zenodo is released under CC-BY-4.0.
