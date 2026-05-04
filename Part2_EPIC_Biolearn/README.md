# Part 2: EPIC Array Aging Clocks Analysis (Biolearn)

## Overview
Epigenetic aging clock analysis using the **Biolearn** Python library on two publicly available GEO DNA methylation datasets. This analysis runs multiple aging clocks, compares their predictions, and visualizes age deviations and correlations across two independent datasets.

**Reference:** Ying et al. (2023). Biolearn, an open-source library for biomarkers of aging. bioRxiv. https://doi.org/10.1101/2023.12.02.569722

---

## Platform
- **Google Colab:** https://colab.google.com (recommended)
- **Language:** Python 3
- **Main Library:** Biolearn

---

## Datasets Used

| GEO ID | Study | Tissue | Samples | Age Range | Platform |
|--------|-------|--------|---------|-----------|----------|
| GSE40279 | Hannum et al. 2013 | Whole blood | 656 | 19-101 yrs | Illumina 450K |
| GSE87571 | Johansson et al. 2017 | Whole blood | 729 | Wide range | Illumina 450K |

### Dataset Descriptions
- **GSE40279:** The landmark Hannum et al. blood methylation aging study. Used to originally build the Hannum aging clock. Contains 656 samples spanning a wide age range.
- **GSE87571:** Large blood methylation dataset used for studying age-related methylation changes across the genome.

---

## Aging Clocks Used

| Clock | Year | Authors | CpG Sites | Tissue | What it Measures |
|-------|------|---------|-----------|--------|-----------------|
| Horvathv1 | 2013 | Horvath | 353 | Pan-tissue | Chronological age (multi-tissue) |
| Hannum | 2013 | Hannum | 71 | Blood | Chronological age (blood) |
| PhenoAge | 2018 | Levine | 513 | Blood | Phenotypic/biological age |
| Lin | 2016 | Lin | 99 | Blood | Chronological age (blood) |
| DunedinPACE | 2022 | Belsky | 173 | Blood | Pace of aging (rate, not age) |

---

## Tasks Completed

### Task 1: Load 2 Complete Datasets
- Loaded GSE40279 and GSE87571 using Biolearn DataLibrary
- Verified sample counts, age ranges, methylation matrix shapes

### Task 2: Run 8+ Aging Clocks
- Successfully ran 5 clocks on both datasets
- Handled output format differences across clocks

### Task 3: Describe Datasets and Clocks
- Full descriptions of biological context, platform, and purpose
- Clock descriptions with CpG site counts and tissue specificity

### Task 4: Correlation Matrix
- Pearson correlation matrix across all clock pairs
- Computed separately for each dataset
- Visualized as annotated heatmaps

### Task 5: Age Deviation Heatmap
- Computed: Age Deviation = Predicted Age - Chronological Age
- Samples sorted by chronological age (youngest to oldest)
- Separate heatmaps for each dataset

### Task 6: Scatter Plots
- Predicted Age vs Chronological Age for each clock
- Includes: regression trend line, identity line (y=x), Pearson r, p-value
- Separate plot panels for each dataset

---

## Key Results

### Correlation Matrix Findings
- Horvathv1, Hannum, PhenoAge, Lin: highly correlated (r = 0.89-0.99)
- DunedinPACE: lower correlation (r = 0.42-0.58) — expected, measures pace not age
- GSE87571 shows even higher inter-clock correlations than GSE40279

### Age Deviation Findings
- Traditional clocks show balanced positive/negative deviations
- DunedinPACE shows consistently large negative deviations (outputs pace units ~1.0, not years)

---

## Results Files

### Plots
| File | Description |
|------|-------------|
| `results/correlation_matrix_dataset1.png` | Clock correlation heatmap - GSE40279 |
| `results/correlation_matrix_dataset2.png` | Clock correlation heatmap - GSE87571 |
| `results/age_deviation_heatmap_dataset1.png` | Age deviation heatmap - GSE40279 |
| `results/age_deviation_heatmap_dataset2.png` | Age deviation heatmap - GSE87571 |
| `results/scatter_plots_dataset1.png` | Scatter plots predicted vs chronological - GSE40279 |
| `results/scatter_plots_dataset2.png` | Scatter plots predicted vs chronological - GSE87571 |

### Data
| File | Description |
|------|-------------|
| `data/predictions_dataset1_GSE40279.csv` | All clock predictions - GSE40279 |
| `data/predictions_dataset2_GSE87571.csv` | All clock predictions - GSE87571 |
| `data/metrics_dataset1_GSE40279.csv` | MAE and Pearson r metrics - GSE40279 |
| `data/metrics_dataset2_GSE87571.csv` | MAE and Pearson r metrics - GSE87571 |

---

## How to Run

### Option 1: Google Colab (Recommended)
1. Upload `notebooks/aging_clocks_analysis.ipynb` to Google Colab
2. Run all cells in order from top to bottom
3. All plots and CSVs will be auto-downloaded at the end

### Option 2: Local
```bash
pip install -r requirements.txt
jupyter notebook notebooks/aging_clocks_analysis.ipynb
```

---

## Requirements
biolearn
pandas
numpy
matplotlib
seaborn
scipy

---

## References
1. Ying et al. (2023). Biolearn. bioRxiv.
2. Hannum et al. (2013). Molecular Cell.
3. Horvath (2013). Genome Biology.
4. Levine et al. (2018). Aging.
5. Belsky et al. (2022). eLife.
