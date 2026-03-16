# Data Source & Download Instructions

## Source

**FAOSTAT — Food and Agriculture Organization of the United Nations**

- Website: https://www.fao.org/faostat
- Section: Trade → Crops and Livestock Products → Detailed Trade Matrix

## How to Download the Raw Data

1. Go to [https://www.fao.org/faostat/en/#data/TM](https://www.fao.org/faostat/en/#data/TM)
2. Set the following filters:
   - **Countries**: Malawi
   - **Elements**: Export Value (USD 1000)
   - **Items**: All items (or filter to specific commodities of interest)
   - **Years**: 1986–2024
3. Click **Download Data** → choose CSV format
4. Save the file and update the CSV Reader node path in the KNIME workflow

## Files in This Folder

| File | Description | Source |
|---|---|---|
| `commodity_clustering.csv` | k-Means cluster output for commodities — includes cluster labels (cluster_0, cluster_1, etc.) and PCA dimension 0 scores. Produced by KNIME Commodity Clustering section. | KNIME output |
| `partner_clustering.csv` | k-Means cluster output for trading partners — includes cluster labels and PCA dimension 0 scores. Produced by KNIME Partner Clustering section. | KNIME output |

## Raw Data File

The raw FAOSTAT export is **not committed to this repository** due to file size and licensing considerations. Download it directly from FAOSTAT using the instructions above.

## Data Notes

- The original data is in **wide format** (one column per year). The KNIME preprocessing pipeline unpivots this to long format for time-series analysis.
- Some years in the 1986–1993 range have sparse or missing values for certain commodity-partner combinations — this is expected given FAOSTAT's data availability for the period.
- The 1997–1998 YoY growth spike (~12,500%) is an artifact of near-zero baseline values in earlier years combined with genuine trade expansion; see the case study for full discussion.
- Export values are denominated in **USD thousands** in the raw FAOSTAT data.
