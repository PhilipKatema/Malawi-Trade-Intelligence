# 🌍 Malawi Trade Intelligence

### Export Analysis · 1986–2024 · FAOSTAT

> A 38-year longitudinal analysis of Malawi's agricultural export economy — covering trade concentration, partner and commodity clustering, year-over-year growth dynamics, rolling volatility, and a 5-year linear regression forecast — built end-to-end in KNIME Analytics Platform.

\---

## 📊 Live Dashboard

[**→ View Interactive Dashboard**](https://philipkatema.github.io/Portfolio/post/malawi_trade_intelligence/Malawi_Trade_Dashboard)

Open in any browser. Includes:

* Commodity \& partner concentration donut charts
* Year-over-year growth line chart (1987–2024)
* Tobacco export forecast through 2029
* Rolling volatility table (Belgium, 3-year window)

\---

## 🔍 Key Findings

|Finding|Detail|
|-|-|
|**Commodity Concentration**|Unmanufactured tobacco = **67.4%** of total export value|
|**Partner Concentration**|Belgium = **23.7%** of total export value|
|**Peak Export Year**|2010 — tobacco exports reached \~**$875M**|
|**Declining Volatility**|Belgium 3-yr std. dev. fell from $71M (2009) → $7.7M (2023)|
|**2029 Forecast**|Linear regression projects tobacco exports at \~**$690M**|

\---

## 🏗️ Project Structure

```
malawi-trade-intelligence/
│
├── README.md                          ← You are here
│
├── data/
│   ├── README\_data.md                 ← Data source info \& download instructions│
├── workflow/
│   ├── README\_workflow.md             ← KNIME workflow documentation
│   └── FAO\_V2.knwf                   ← KNIME workflow export (place here)
│
├── outputs/
│   ├── top\_10\_products\_chart.png
│   ├── top\_10\_products\_table.png
│   ├── top\_10\_partners\_chart.png
│   ├── top\_10\_partners\_table.png
│   ├── yoy\_growth\_chart.png
│   ├── yoy\_growth\_table.png
│   ├── rolling\_volatility\_belgium.png
│   ├── regression\_forecast\_chart.png
│   └── regression\_forecast\_table.png
││
└── docs/
    └── Malawi\_Trade\_Intelligence\_Case\_Study.pdf  ← Full case study PDF
```

\---

## 🛠️ Tools \& Methods

|Tool / Method|Role|
|-|-|
|**KNIME Analytics Platform**|End-to-end pipeline: ingestion, preprocessing, analysis, visualization|
|**k-Means Clustering**|Unsupervised segmentation of partners and commodities|
|**PCA**|Dimensionality reduction for cluster visualization|
|**Linear Regression**|5-year export value forecast (KNIME Learner/Predictor nodes)|
|**Moving Aggregator**|3-year rolling standard deviation for volatility analysis|
|**Chart.js**|Interactive web dashboard visualization|

\---

## 📥 Data Source

**FAOSTAT — Food and Agriculture Organization of the United Nations**

* URL: [https://www.fao.org/faostat](https://www.fao.org/faostat)
* Dataset: Trade → Crops and Livestock Products (Detailed Trade Matrix)
* Reporting Country: Malawi
* Trade Flow: Export
* Time Range: 1986–2024
* Unit: USD (1000 US$)

\---

## ⚙️ Running the KNIME Workflow

1. Download and install [KNIME Analytics Platform](https://www.knime.com/downloads) (free)
2. Download the raw FAOSTAT data (see `data/README\_data.md`)
3. Import `workflow/FAO\_V2.knwf` via **File → Import KNIME Workflow**
4. Update the CSV Reader node path to point to your downloaded data file
5. Execute all (`Ctrl+Shift+F7`) or run section by section

See [`workflow/README\_workflow.md`](https://github.com/PhilipKatema/Malawi-Trade-Intelligence/workflow/README_workflow.md) for detailed node-by-node documentation.

\---

## 📁 Key Output Files

|File|Description|
|-|-|
|`docs/Malawi\_Trade\_Intelligence\_Case\_Study.pdf`|Full 9-section professional case study|
|`dashboard/Malawi\_Trade\_Dashboard.html`|Interactive browser dashboard|

\---

## 👤 Author

**Philip Katema**  
Data Analytics | Business Intelligence | Data Engineering  
[LinkedIn](https://www.linkedin.com/in/phillip-katema-419632b3/) · [Portfolio](https://github.com/PhilipKatema)

\---

## 📄 License

Data sourced from FAOSTAT is subject to [FAO's terms of use](http://www.fao.org/contact-us/terms/en/).  
Analysis, code, and documentation in this repository © Philip Katema, 2025.

