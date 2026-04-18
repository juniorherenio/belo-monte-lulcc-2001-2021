# Belo Monte HPP — Land Use and Land Cover Change (2001–2021)

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19644591.svg)](https://doi.org/10.5281/zenodo.19644591)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

Data and analytical scripts for:

> **Silva, P.R.P., Beskow, T.L.C., Campos, H.R.P., Albuquerque, D.P., Gonçalves Júnior, D.H., Torres Neto, F.F., & Terra, F.S.** (2025). Pre-construction deforestation pressure and reservoir-driven hydrological reorganization in the Belo Monte HPP direct influence area, Brazilian Amazon (2001–2021): a multitemporal geospatial analysis. Manuscript submitted for publication.

---

## Overview

This repository contains the classified land cover shapefiles, transition matrices, and Python analytical scripts used in the study. The analysis covers the **431,357-ha direct influence area** of the Belo Monte Hydroelectric Plant (Xingu basin, Pará, Brazil) across five epochs: **2001, 2006, 2011, 2016, and 2021**.

Key findings:
- **86.3%** of total forest loss (65,761 ha) occurred before construction began (2001–2011)
- Forest loss rate reached a **20-year minimum** during peak construction (CAGR −0.31%/yr, 2011–2016)
- **Pasture expansion** drove 58–99% of deforestation across all phases
- BAU scenario projects **179,938 ha** of forest by 2030 vs. 141,560 ha under the pre-project rate

---

## Repository structure

```
belo-monte-lulcc-2001-2021/
│
├── data/
│   ├── shapefiles/
│   │   ├── 2001.shp  (.dbf .prj .shx)   ← classified land cover 2001
│   │   ├── 2006.shp  (.dbf .prj .shx)
│   │   ├── 2011.shp  (.dbf .prj .shx)
│   │   ├── 2016.shp  (.dbf .prj .shx)
│   │   ├── 2021.shp  (.dbf .prj .shx)
│   │   └── AREA_OF_INTEREST.shp (.dbf .prj .shx)  ← study boundary
│   └── Transition_Matrices_2001-2022.xlsx
│
├── scripts/
│   └── belo_monte_lulcc_analysis.ipynb   ← full analysis pipeline
│
└── README.md
```

---

## Data sources

| Dataset | Source | License |
|---|---|---|
| Landsat 5/7/8 imagery | [Google Earth Engine](https://earthengine.google.com) / USGS | Public domain |
| MapBiomas Collection 9 | [data.mapbiomas.org](https://data.mapbiomas.org) (doi:[10.58053/MapBiomas/VEJDZC](https://doi.org/10.58053/MapBiomas/VEJDZC)) | CC BY 4.0 |
| Study boundary | Eletrobras (2009) EIA — Belo Monte HPP | — |
| Classified shapefiles (this repo) | This study | CC BY 4.0 |

The Landsat imagery is publicly available via Google Earth Engine. MapBiomas Collection 9 is automatically downloaded by Block 06 of the notebook.

---

## Land cover classes

| gridcode | Class |
|---|---|
| 3 | Forest |
| 6 | Floodplain (várzea) |
| 11 | Flooded Field (campo alagado) |
| 12 | Natural Field |
| 15 | Pasture |
| 24 | Urban Area |
| 33 | Water |
| 41 | Temporary Crop |

Class definitions follow [MapBiomas Brasil nomenclature](https://mapbiomas.org/en/legend-codes).  
Coordinate system: **SIRGAS 2000 / UTM Zone 22S (EPSG:31982)**.

---

## Notebook structure

The notebook `belo_monte_lulcc_analysis.ipynb` is organized in six sequential blocks:

| Block | Description | Main outputs |
|---|---|---|
| **01** | Data loading and area calculation | `master_areas_ha.csv`, `master_areas_pct.csv` |
| **02** | CAGR analysis and Mann-Kendall trends | `cagr_by_phase.csv`, Figure 4, Figure 5 |
| **03** | Transition matrix analysis | `forest_loss_flows.csv`, Figure 5, Figure S1 |
| **04** | Pearson correlation (raw + first-difference) | `correlation_firstdiff_pearson.csv`, Figure S2, S3 |
| **05** | Scenario projections to 2030 | `scenario_projections.csv`, Figure 6 |
| **06** | External validation (MapBiomas Col. 9) | `mb_cagr_comparison.csv`, Figure 2, Figure S4 |

---

## How to run

### Requirements

```bash
pip install geopandas pandas numpy matplotlib scipy scikit-learn openpyxl pymannkendall requests
```

### Steps

1. Clone the repository:
```bash
git clone https://github.com/juniorherenio/belo-monte-lulcc-2001-2021.git
cd belo-monte-lulcc-2001-2021
```

2. Update `BASE_DIR` at the top of each block to match your local path:
```python
BASE_DIR = Path(r'C:\your\path\to\belo-monte-lulcc-2001-2021')
```

3. Open and run the notebook sequentially:
```bash
jupyter notebook scripts/belo_monte_lulcc_analysis.ipynb
```

Blocks run independently — each block reloads data from CSV if not running in an active session. Block 06 downloads MapBiomas Collection 9 automatically (~68 MB, requires internet connection).

---

## Figures

| Figure | Description | Location |
|---|---|---|
| Fig. 1 | Study area map — 2001 baseline | QGIS/Inkscape (not in notebook) |
| Fig. 2 | MapBiomas validation | `outputs/Figure_2_mapbiomas_validation.png` |
| Fig. 3 | Multitemporal panel 2001–2016 | QGIS/Inkscape (not in notebook) |
| Fig. 4 | Annual deforestation rate by phase | `outputs/Figure_4_forest_dynamics_by_phase.png` |
| Fig. 5 | Forest conversion flows | `outputs/Figure_5_forest_loss_flows.png` |
| Fig. 6 | Scenario projections 2021–2030 | `outputs/Figure_6_scenario_projections.png` |
| Fig. S1 | Transition matrix heatmaps | `outputs/Figure_S1_transition_heatmaps.png` |
| Fig. S2 | Correlation heatmaps | `outputs/Figure_S2_correlation_heatmaps.png` |
| Fig. S3 | Key pairwise scatter plots | `outputs/Figure_S3_scatter_key_pairs.png` |
| Fig. S4 | Normalized trajectories | `outputs/Figure_S4_normalized_trajectories.png` |

---

## Citation

If you use this dataset or scripts, please cite the article and this repository:

**Article:**
> Silva, P.R.P., Beskow, T.L.C., Campos, H.R.P., Albuquerque, D.P., Gonçalves Júnior, D.H., Torres Neto, F.F., & Terra, F.S. (2025). Pre-construction deforestation pressure and reservoir-driven hydrological reorganization in the Belo Monte HPP direct influence area, Brazilian Amazon (2001–2021): a multitemporal geospatial analysis. Manuscript submitted for publication.

**Dataset and scripts:**
> Silva, P.R.P. et al. (2025). *Belo Monte HPP — Land Use and Land Cover Change (2001–2021)* [Dataset and scripts]. Zenodo. https://doi.org/10.5281/zenodo.19644591

**MapBiomas Collection 9:**
> Souza, C.M., Jr., et al. (2020). Reconstructing three decades of land use and land cover changes in Brazilian biomes with Landsat archive and Earth Engine. *Remote Sensing*, 12(17), 2735. https://doi.org/10.3390/rs12172735

---

## License

The classified shapefiles and scripts in this repository are released under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).

Landsat imagery is public domain (USGS). MapBiomas Collection 9 is licensed under CC BY 4.0.

---

## Contact

**Deurimar Júnior**  
Universidade Federal do Espírito Santo - CEUNES, São Mateus, ES, Brazil  
Email: juniorherenio@gmail.com
