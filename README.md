# 🌍 Global Happiness & Economic Indicators - Hierarchical Clustering

An unsupervised machine learning project that groups **159 countries** into meaningful development tiers using **Agglomerative Hierarchical Clustering (Ward linkage)** on economic and well-being indicators inspired by the World Happiness Report.

![Python](https://img.shields.io/badge/Python-3.9%2B-blue)
![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-orange)
![Status](https://img.shields.io/badge/status-complete-brightgreen)

---

## 📌 Table of Contents
- [Project Overview](#-project-overview)
- [Dataset](#-dataset)
- [Project Workflow](#-project-workflow)
- [Key Results](#-key-results)
- [Cluster Profiles](#-cluster-profiles)
- [Visual Highlights](#-visual-highlights)
- [Repository Structure](#-repository-structure)
- [Installation & Usage](#-installation--usage)
- [Limitations](#-limitations)
- [Future Work](#-future-work)

---

## 🎯 Project Overview

**Objectives**
1. Explore the dataset with 12 different visualizations (EDA).
2. Clean and preprocess the data (missing values, aggregation, scaling).
3. Apply Agglomerative Hierarchical Clustering to group countries.
4. Validate the clusters with internal metrics and interpret/profile each group.

**Tech stack:** Python · Pandas · NumPy · Matplotlib · Seaborn · SciPy · scikit-learn

---

## 📊 Dataset

File: `global_happiness_economic_indicators.csv`
**3,180 rows × 12 columns** - country-year observations from **2004 to 2023**.

> ⚠️ The dataset is **synthetic** (realistic but generated), modelled on the World Happiness Report methodology. Results illustrate the technique and should not be read as real-world findings about any specific country.

| Column | Description |
|---|---|
| `Country` | Country name |
| `Region` | Geographic / economic region (10 regions) |
| `Year` | Year of observation (2004–2023) |
| `GDP_per_capita` | GDP per capita (USD) |
| `Social_support` | Social support index (0–1) |
| `Healthy_life_expectancy` | Healthy life expectancy at birth (years) |
| `Freedom_to_make_choices` | Freedom to make life choices index (0–1) |
| `Generosity` | Generosity index |
| `Perceptions_of_corruption` | Corruption perception index (0–1, higher = lower perceived corruption) |
| `Unemployment_rate` | Unemployment rate (%) |
| `Population_millions` | Population (millions) |
| `Happiness_score` | Overall happiness score (2–8 scale) |

---

## 🔄 Project Workflow

1. **Import libraries & setup** — pandas, seaborn, SciPy, scikit-learn.
2. **Load data** — inspect shape, dtypes and summary statistics.
3. **Data cleaning** — 31 missing values each in `GDP_per_capita`, `Generosity` and `Unemployment_rate` were imputed with the **region-wise median**; no duplicate rows found.
4. **Exploratory Data Analysis** — 12 plots: histograms, boxplot, violin plot, scatter plots, correlation heatmap, line trend, pie chart, bubble chart and pairplot.
5. **Feature preparation** — data aggregated to **one row per country** (mean across years) → 159 countries; 8 features standardized with `StandardScaler`.
6. **Dendrogram** — Ward linkage using SciPy to visualize the merge hierarchy.
7. **Choosing k** — silhouette scores for k = 2…8 and merge-distance view.
8. **Final model** — `AgglomerativeClustering(n_clusters=4, linkage="ward")`.
9. **Evaluation** — Silhouette, Davies-Bouldin and Calinski-Harabasz scores.
10. **Visualization** — 2-D PCA projection and region-vs-cluster cross-check.
11. **Profiling** — mean feature values per cluster and interpretation.

---

## 📈 Key Results

**EDA insights**
- Happiness is strongly correlated with **GDP per capita (0.83)**, **healthy life expectancy (0.83)** and **social support (0.79)**.
- Unemployment is negatively correlated with happiness (**−0.61**).
- Population shows almost no correlation with any indicator.
- Oceania, North America and Western Europe have the highest median happiness; South Asia and Sub-Saharan Africa the lowest.

**Model evaluation (k = 4)**

| Metric | Value | Interpretation |
|---|---|---|
| Silhouette Score | 0.295 | Moderate separation (higher is better) |
| Davies-Bouldin Index | 1.404 | Lower is better |
| Calinski-Harabasz | 110.6 | Higher is better |
| PCA (2 components) | 76.3% variance | PC1 = 62.9%, PC2 = 13.4% |

**Silhouette score by k:** k=2 → 0.421 · k=3 → 0.382 · **k=4 → 0.295** · k=5 → 0.275 · k=6 → 0.206 · k=7 → 0.157 · k=8 → 0.140

> k = 2 scores highest statistically, but **k = 4 was chosen** because it gives richer, more interpretable development tiers (k = 3 is a reasonable alternative).

---

## 🧩 Cluster Profiles

Mean values per cluster (original scale):

| Cluster | Countries | GDP / capita | Life exp. | Freedom | Corruption idx | Unemployment | Happiness | Main regions |
|:-:|:-:|--:|--:|--:|--:|--:|--:|---|
| **1** — High-income, high-wellbeing | 35 | $55,652 | 79.8 | 0.87 | 0.62 | 5.7% | **6.76** | Western Europe, Oceania, North America |
| **0** — Upper-middle / emerging | 56 | $22,630 | 75.1 | 0.70 | 0.35 | 6.4% | 5.39 | Latin America, Eastern Europe, Southeast Asia |
| **3** — Wealthy but constrained | 20 | $34,574 | 74.2 | 0.58 | 0.32 | 8.8% | 5.36 | Middle East (mostly) |
| **2** — Lower-income, development challenges | 48 | $5,327 | 63.0 | 0.61 | 0.28 | 10.0% | **3.83** | Sub-Saharan Africa, South Asia |

**Reading the clusters:** Cluster 3 is the most interesting — GDP is well above Cluster 0, yet freedom, corruption perception and happiness are no higher, and unemployment is notably worse. Wealth alone does not translate into well-being.

---

## 📁 Repository Structure

```
global-happiness-hierarchical-clustering/
│
├── global_happiness_economic_indicators.csv
├── notebooks/
│   └── Global_Happiness_Hierarchical_Clustering.ipynb
├── docs/
│   └── Project_Documentation.docx
├── requirements.txt
├── .gitignore
└── README.md
```

> The notebook currently reads the CSV from its own folder. If you use the structure above, change the load line to
> `pd.read_csv("../global_happiness_economic_indicators.csv")`.

---

## ⚙️ Installation & Usage

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/global-happiness-hierarchical-clustering.git
cd global-happiness-hierarchical-clustering

# 2. (Optional) create a virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Launch the notebook
jupyter notebook notebooks/Global_Happiness_Hierarchical_Clustering.ipynb
```

**`requirements.txt`**
```
numpy
pandas
matplotlib
seaborn
scipy
scikit-learn
jupyter
```

---

## ⚠️ Limitations
- The data is **synthetic**, so cluster–region patterns are illustrative only.
- Silhouette score (0.295) indicates moderately overlapping clusters, especially between Clusters 0 and 3.
- Averaging across 20 years hides how individual countries change over time.
- Ward linkage assumes roughly spherical, similarly sized clusters.

## 🚀 Future Work
- Compare linkage methods (average, complete) using cophenetic correlation.
- Cluster at the country-year level to track countries moving between clusters.
- Benchmark against K-Means, DBSCAN or Gaussian Mixture Models.
- Add real indicators (education, internet penetration) from the actual World Happiness Report.

---

## 👤 Author
**Akhilesh** · [GitHub](https://github.com/akhileshcoding08) · [LinkedIn](https://linkedin.com/in/akhileshhadke)

⭐ If you found this project useful, consider giving it a star!
