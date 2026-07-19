# Impact of 5G Signal Strength on Voice Quality Across n1, n28, and n78

**Author:** Paweł Cebula

## Executive summary

This project investigates how 5G signal strength, measured by Reference Signal Received Power (RSRP), relates to perceived voice quality, measured by Mean Opinion Score (MOS). The analysis uses field-test data from one 5G collection and compares three radio layers: n28 (coverage), n1 (mixed coverage/capacity), and n78 (capacity).

The project includes data cleaning, exploratory data analysis, feature engineering, visualizations, correlation analysis, and a baseline regression model. The main goal is to determine whether weaker RSRP is associated with lower MOS and whether this relationship differs between frequency bands.

## Rationale

Mobile users experience network quality through service performance rather than radio measurements alone. A strong relationship between RSRP and MOS would help identify radio conditions associated with degraded voice quality. Comparing n28, n1, and n78 is useful because these bands serve different coverage and capacity roles.

## Research Question

How does 5G signal strength, measured by RSRP, affect voice quality, measured by MOS, across the n1, n28, and n78 bands within one field-test collection?

Supporting questions:

1. Does average MOS differ between n1, n28, and n78?
2. Is the relationship between RSRP and MOS different for each band?
3. Does adding SINR improve the ability to predict MOS?
4. Which radio variables are most useful for explaining MOS variation?

## Data Sources

The dataset is exported from internal 5G field-test logs. Voice-quality data and radio measurements are joined by `SessionId` and `TestId`.

Expected variables:

- `MOS` — voice-quality score.
- `RSRP` or `RSRP Rx` — received signal strength.
- `Band` or `RFBand` — 5G band identifier.
- `SINR` or `SINR Rx` — signal quality, when available.
- `Timestamp` — measurement time.
- `SessionId` and `TestId` — identifiers used for joining and validation.
- `CollectionName` — field-test collection.

Only records from one selected 5G collection and bands n1, n28, and n78 are included.

## Methodology

1. Import and inspect the exported CSV dataset.
2. Standardize column names and data types.
3. Remove duplicate records.
4. Remove records missing MOS, RSRP, or band.
5. Apply domain checks:
   - MOS between 1 and 5.
   - RSRP between -160 and -40 dBm.
   - SINR between -30 and 50 dB, when available.
6. Analyze possible outliers by band using the IQR method.
7. Create RSRP range categories for comparison.
8. Compare distributions and summary statistics across bands.
9. Visualize:
   - sample counts,
   - MOS and RSRP distributions,
   - MOS by band,
   - MOS versus RSRP,
   - MOS by RSRP range and band,
   - numeric-variable correlations.
10. Build a baseline regression model using RSRP, band, and SINR when available.
11. Evaluate the model using MAE, RMSE, and R².

MAE is the primary metric because it expresses prediction error directly in MOS points. RMSE is also reported because it penalizes large prediction errors more strongly. R² shows how much MOS variation is explained by the model.

## Results

The final numeric findings must be copied from the last section of the executed notebook.

Replace the bracketed values after running the analysis:

- Clean observations analyzed: **[N]**
- Observations by band: **[n1 = ...; n28 = ...; n78 = ...]**
- Band with the highest average MOS: **[band]**
- Strongest absolute RSRP–MOS correlation: **[band, correlation]**
- Mean-prediction baseline MAE: **[value]**
- Linear-regression MAE: **[value]**
- Linear-regression RMSE: **[value]**
- Linear-regression R²: **[value]**

Interpretation template:

> The analysis found that MOS **[increased/decreased/showed a weak relationship]** as RSRP improved. The relationship was strongest on **[band]** and weakest on **[band]**. The regression model **[did/did not]** improve on the mean-prediction baseline. This indicates that radio signal strength **[alone is insufficient/provides meaningful information]** for explaining voice quality. SINR, handovers, codec behavior, packet loss, and call-state events should be considered in the next modeling stage.

## Next steps

- Validate results using additional collections and routes.
- Add SINR, RSRQ, handover count, serving-cell changes, codec, packet loss, and call-state information.
- Test non-linear models such as Random Forest and Gradient Boosting.
- Use grouped validation by session or route to reduce leakage.
- Investigate low-MOS cases individually in radio and signaling logs.
- Compare results between operators, devices, and network configurations.

## Outline of project

- [Initial EDA and baseline model](notebooks/01_initial_eda.ipynb)

## Repository structure

```text
project/
├── README.md
├── data/
│   └── mos_rsrp_5g.csv
└── notebooks/
    └── 01_initial_eda.ipynb
```

## Contact and Further Information

For questions about the analysis, contact the project author through the repository.
