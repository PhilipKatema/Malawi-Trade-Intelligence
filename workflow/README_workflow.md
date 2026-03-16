# KNIME Workflow Documentation

## Overview

The KNIME workflow (`FAO\_V2.knwf`) is structured as a **modular pipeline** with a shared Data Preprocessing branch feeding six independent analytical sections.

\---

## Workflow Architecture

```
\[CSV Reader]
     │
\[Data Preprocessing]──────────────────────────────────────────────┐
     │                                                             │
     ├──► \[Concentration Metrics – Top Commodity]                 │
     │         GroupBy → Sorter → Row Filter →                    │
     │         Column Appender → Renamer → Math Formula → Pie     │
     │                                                             │
     ├──► \[Concentration Metrics – Top Partner]                   │
     │         (same pattern as above, filtered by partner)        │
     │                                                             │
     ├──► \[Partner Clustering]                                     │
     │         Pivot → Missing Value → Normalizer →               │
     │         k-Means → Color Manager → PCA → Scatter Plot       │
     │                                                             │
     ├──► \[Commodity Clustering]                                   │
     │         (same pattern as Partner Clustering)                │
     │                                                             │
     ├──► \[Growth Features (YoY)]                                  │
     │         GroupBy → Lag Column → Math Formula → Line Plot     │
     │                                                             │
     ├──► \[Rolling Volatility – 3 Years]                          │
     │         Row Filter → Moving Aggregator → Line Plot          │
     │                                                             │
     └──► \[Next 5 Years Value Prediction (Linear Regression)]
               Date→String → GroupBy → Table Partitioner →
               LR Learner → Regression Predictor → Concatenate →
               Column Renamer → Line Plot
```

\---

## Section-by-Section Node Documentation

### 1\. Data Preprocessing

|Node|Purpose|
|-|-|
|CSV Reader|Load raw FAOSTAT wide-format file|
|Row Filter|Keep only Malawi export records|
|Column Filter|Remove metadata columns not needed for analysis|
|Unpivot|Convert wide format (one col per year) → long format (Year, Value rows)|
|Column Renamer|Standardize column names|
|String Manipulation|Clean country/commodity name strings|
|String to Date\&Time|Parse year column to KNIME Date type|
|Row Filter (2nd)|Remove null/zero value rows|
|Column Auto Type Cast|Ensure numeric columns typed correctly|
|Column Filter (2nd)|Final column selection|
|Math Formula|Compute derived metrics (e.g., aggregated export value)|

### 2\. Concentration Metrics

Both sections (Top Commodity, Top Partner) follow the same pattern:

|Node|Purpose|
|-|-|
|Table Row to Variable|Extract total export value as flow variable|
|GroupBy|Sum export values by item/partner|
|Constant Value Row Appender|Add a "Total" row|
|GroupBy (2nd)|Further aggregation|
|Sorter|Sort descending by export value|
|Row Filter|Keep top 10 only|
|Column Appender|Add percentage share column|
|Column Renamer|Clean output column names|
|Math Formula|Calculate % share = value / total × 100|
|Pie Chart|Render concentration visualization|

### 3\. Clustering (Partner \& Commodity)

|Node|Purpose|
|-|-|
|Pivot|Convert long→wide (one row per partner/commodity, year columns as features)|
|Missing Value|Impute nulls with 0|
|Normalizer|Z-score standardize all year columns|
|k-Means|Cluster assignment (configured k = 2–4)|
|Color Manager|Assign colors to clusters for visualization|
|PCA|Reduce to 2 principal components|
|Scatter Plot|Visualize clusters in PCA space|

**Output:** CSV with cluster label and PCA dimension 0 score per entity.

### 4\. Growth Features (YoY)

|Node|Purpose|
|-|-|
|GroupBy|Sum all exports by year|
|Lag Column|Create prior-year value column|
|Math Formula|YoY% = (current − prior) / prior × 100|
|Line Plot|Render growth time series|

### 5\. Rolling Volatility

|Node|Purpose|
|-|-|
|Row Filter|Filter to Belgium records only|
|Moving Aggregator|3-year rolling window → std. deviation of export value|
|Line Plot|Render volatility trend|

### 6\. Linear Regression Forecast

|Node|Purpose|
|-|-|
|Date\&Time to String|Convert year to numeric feature|
|String to Number|Ensure year is numeric type|
|GroupBy|Aggregate annual tobacco export values|
|Table Partitioner|Split into train (historical) / predict (2025–2029) sets|
|Linear Regression Learner|Train model on historical data|
|Regression Predictor|Apply model to future years|
|Constant Value Column Appender|Tag rows as "Historical" or "Forecast"|
|Column Renamer|Clean output column names|
|Concatenate|Merge historical + forecast into single table|
|Line Plot|Render combined chart|

\---

## Configuration Notes

* **k-Means k value:** Tested with k=2, 3, 4. Final results use k=3 (cluster\_0, cluster\_1, cluster\_2) for interpretability.
* **Normalizer:** Min-max normalization produced similar cluster structures; z-score used for final output.
* **Linear Regression:** Single predictor (Year as numeric). R² noted but not the primary output metric — the visual trend line is the analytical deliverable.
* **Rolling Window:** Moving Aggregator set to window size = 3, aggregation = Standard Deviation.

