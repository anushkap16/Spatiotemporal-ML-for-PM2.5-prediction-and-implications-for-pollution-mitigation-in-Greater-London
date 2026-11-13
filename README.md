# Spatiotemporal machine learning for PM2.5 prediction and implications for pollution mitigation in Greater London

Code accompanying the manuscript by Anushka Pandey (Cambridge Centre for International Research).

**Data:** (the data are too large for GitHub and are not stored in this repository).

## What this code does

The analysis has three stages, run in order:

| Stage | Purpose | Script / notebook |
|---|---|---|
| 1. Initial experiment | Trains the seven baseline models (Elastic Net, Random Forest, XGBoost, LightGBM, CatBoost, MLP, and RNN where applicable) using environmental, geographic and temporal features only. Compares the spatial setting (random 80/20 station split) with the temporal setting (chronological 80/20 split), and tests winsorisation caps (90th to 99.5th percentile). | `[initial_experiment.py]` |
| 2. Lagged PM2.5 inputs | Adds PM2.5 from the previous *L* hours at the same location (*L* = 0 to 24), selects the best lookback for each model, and tunes hyperparameters (randomised search, then grid search at the 99.5% cap). Produces the final Random Forest (14-hour lookback) and SHAP values. | `[lagged_inputs.py]` |
| 3. Counterfactual analysis | Scales major-road length, total road length, traffic AADF and NDVI by up to ±30% (current and lagged copies), holds lagged PM2.5 fixed, and reports mean change in predicted PM2.5 for each borough and Central London. | `[counterfactual.py]` |

## Setup

1. Download `MainData.csv` and `london_boroughs.csv` from the Zenodo record above and place them in `data/`.
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Run the stages in order:
   ```bash
   python [initial_experiment.py]
   python [lagged_inputs.py]
   python [counterfactual.py]
   ```

## Repository structure

```
├── data/
├── [initial_experiment.py]
├── [lagged_inputs.py]
├── [counterfactual.py]
├── requirements.txt
├── LICENSE
└── README.md
```

## Citation

Please cite the paper and the dataset if used.
