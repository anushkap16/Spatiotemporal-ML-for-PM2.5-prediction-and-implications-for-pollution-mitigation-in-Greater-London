# Code for: Spatiotemporal machine learning for PM2.5 prediction and implications for pollution mitigation in Greater London

Code accompanying the manuscript by Anushka Pandey (Cambridge Centre for International Research).
**Data:** https://doi.org/10.5281/zenodo.24153394 (the data are too large for GitHub and are not stored in this repository).

## What this code does

The analysis has three stages, run in order:

| Stage | Purpose | Script / notebook |
|---|---|---|
| 1. Initial experiment | Trains the seven baseline models (Elastic Net, Random Forest, XGBoost, LightGBM, CatBoost, MLP, and RNN where applicable) using environmental, geographic and temporal features only. Compares the spatial setting (random 80/20 station split) with the temporal setting (chronological 80/20 split), and tests winsorisation caps (90th to 99.5th percentile). | `[01_initial_experiment.py]` |
| 2. Lagged PM2.5 inputs | Adds PM2.5 from the previous *L* hours at the same location (*L* = 0 to 24), selects the best lookback for each model, and tunes hyperparameters (randomised search, then grid search at the 99.5% cap). Produces the final Random Forest (14-hour lookback) and SHAP values. | `[02_lagged_inputs.py]` |
| 3. Counterfactual analysis | Scales major-road length, total road length, traffic AADF and NDVI by up to ±30% (current and lagged copies), holds lagged PM2.5 fixed, and reports mean change in predicted PM2.5 for each borough and Central London. | `[03_counterfactual.py]` |

## Setup

1. Download `MainData.csv` and `london_boroughs.csv` from the Zenodo record above and place them in `data/`.
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
   Main libraries: scikit-learn, PyTorch, XGBoost, LightGBM, CatBoost, SHAP, pandas, NumPy. [Pin the versions you used and the Python version.]
3. Run the stages in order:
   ```bash
   python [01_initial_experiment.py]
   python [02_lagged_inputs.py]
   python [03_counterfactual.py]
   ```

## Outputs

[Describe where results are saved, e.g. `results/` for tables and `figures/` for plots, and which output corresponds to which table or figure in the paper.]

## Reproducibility notes

- Splits are chronological: the first 80% of observations (through 17 April 2025) are training data, the rest test data.
- Normalisation statistics and winsorisation caps are computed on the training data only.
- Random seed: [state it]. Approximate run time and hardware: [state it].
- The lookback, winsorisation level and hyperparameters were selected using the test period (see the limitations in the paper).

## Repository structure

```
├── data/                 (place downloaded data here; not tracked)
├── [01_initial_experiment.py]
├── [02_lagged_inputs.py]
├── [03_counterfactual.py]
├── requirements.txt
├── LICENSE
└── README.md
```

## Citation

Please cite the paper [citation to be added once published] and the dataset:

> Pandey, A. (2026). Dataset for: Spatiotemporal machine learning for PM2.5 prediction and implications for pollution mitigation in Greater London [Dataset]. Zenodo. https://doi.org/10.5281/zenodo.24153394

## Licence

[MIT, or another licence of your choice.]
