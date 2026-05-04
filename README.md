# DNA Methylation Analysis Project
## Epigenetic Biomarker Analysis: Cancer & Aging

![Biology](https://img.shields.io/badge/Field-Epigenetics-blue)
![Platform](https://img.shields.io/badge/Platform-Galaxy%20%7C%20Python-green)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

---

## Table of Contents
- [Project Overview](#project-overview)
- [Biological Background](#biological-background)
- [Repository Structure](#repository-structure)
- [Part 1: WGBS Analysis](#part-1-wgbs-analysis)
- [Part 2: Aging Clocks](#part-2-aging-clocks)
- [Key Results](#key-results)
- [How Parts Connect](#how-parts-connect)
- [Requirements](#requirements)
- [References](#references)

---

## Project Overview

This project investigates **DNA methylation** — the addition of methyl groups to cytosine bases at CpG dinucleotides — as a powerful epigenetic biomarker. DNA methylation plays critical roles in gene regulation, development, disease, and aging. This project explores it through two independent but complementary analyses:

| Feature | Part 1: WGBS | Part 2: EPIC Array |
|---------|-------------|-------------------|
| **Method** | Whole Genome Bisulfite Sequencing | Illumina 450K Array |
| **Platform** | Galaxy Europe (usegalaxy.eu) | Python / Google Colab |
| **Tools** | Falco, bwameth, MethylDackel, deeptools | Biolearn Python library |
| **Data** | Lin et al. 2015 breast cancer WGBS | GEO: GSE40279, GSE87571 |
| **Focus** | Cancer-associated methylation changes | Epigenetic aging clock predictions |
| **Output** | QC reports, alignment, coverage plots | Correlation matrices, heatmaps, scatter plots |

---

## Biological Background

### What is DNA Methylation?
DNA methylation is an epigenetic modification where a methyl (-CH3) group is added to the 5th carbon of cytosine, predominantly at CpG dinucleotides. It is maintained by DNA methyltransferases (DNMTs) and plays roles in:
- Gene silencing and activation
- X-chromosome inactivation
- Genomic imprinting
- Cancer development
- Biological aging

### Why is it Important?
- **Cancer:** Aberrant methylation patterns drive oncogene activation and tumor suppressor silencing
- **Aging:** Methylation patterns change predictably with age, forming the basis of epigenetic clocks
- **Biomarker potential:** Can be measured from blood, is stable, and reflects disease and aging state

---

```
## Repository Structure
DNA-Methylation-Analysis/
│
├── README.md                          <- This file
│
├── Part1_WGBS_Galaxy/
│   ├── README.md                      <- Detailed Part 1 documentation
│   ├── results/
│   │   ├── falco_reports/             <- QC reports (Falco)
│   │   │   ├── falco_subset1_report.html
│   │   │   └── falco_subset2_report.html
│   │   ├── methylation_bias_plots/    <- MethylDackel bias SVGs
│   │   │   ├── methyldackel_bias_top_strand.svg
│   │   │   └── methyldackel_bias_bottom_strand.svg
│   │   ├── methylation_extraction/    <- CpG methylation levels
│   │   │   └── methylation_levels.tabular
│   │   └── visualization/            <- computeMatrix + plotProfile
│   │       ├── plotProfile_CpG_islands.png
│   │       └── methylation_heatmap_data.tabular
│   └── workflow/
│       └── galaxy_steps.md           <- Step-by-step Galaxy workflow
│
├── Part2_EPIC_Biolearn/
│   ├── README.md                     <- Detailed Part 2 documentation
│   ├── notebooks/
│   │   └── aging_clocks_analysis.ipynb
│   ├── results/
│   │   ├── correlation_matrix_dataset1.png
│   │   ├── correlation_matrix_dataset2.png
│   │   ├── age_deviation_heatmap_dataset1.png
│   │   ├── age_deviation_heatmap_dataset2.png
│   │   ├── scatter_plots_dataset1.png
│   │   └── scatter_plots_dataset2.png
│   ├── data/
│   │   ├── predictions_dataset1_GSE40279.csv
│   │   ├── predictions_dataset2_GSE87571.csv
│   │   ├── metrics_dataset1_GSE40279.csv
│   │   └── metrics_dataset2_GSE87571.csv
│   └── requirements.txt
│
└── docs/
└── project_report.md             <- Full project report

---
```

## Part 1: WGBS Analysis

### Overview
Whole Genome Bisulfite Sequencing (WGBS) analysis of breast cancer methylation data using the Galaxy Training Network (GTN) tutorial pipeline.

### Dataset
- **Source:** Lin et al. 2015 (PLOS ONE), Zenodo DOI: 10.5281/zenodo.557099
- **Samples:** Breast tumor (BT089, BT126, BT198) vs Normal breast (NB1, NB2)
- **Reference genome:** hg19 (GRCh37)

### Pipeline Steps

| Step | Tool | Purpose | Result |
|------|------|---------|--------|
| 1 | Data Upload | Load FASTQ files | subset_1.fastq, subset_2.fastq |
| 2 | Falco | Quality control | PASS - 458,003 reads, %GC ~26% |
| 3 | bwameth | Bisulfite alignment | BAM files ~60 MB each |
| 4 | MethylDackel (mbias) | Methylation bias check | No trimming required |
| 5 | MethylDackel (extract) | CpG methylation extraction | bedGraph format |
| 6 | bamCoverage | BAM to bigwig | 1x normalized bigwig |
| 7 | computeMatrix | Matrix around CpG islands | ±500bp reference-point |
| 8 | plotProfile | Coverage visualization | Enrichment at CpG centers |

### Key Findings
- Low %GC (~26%) confirmed successful bisulfite conversion (C→T)
- High-quality reads with tight quality score distribution
- Clear coverage enrichment at CpG island centers
- Methylation levels extracted for chromosomes 1, 10, and 11

---

## Part 2: Aging Clocks Analysis

### Overview
Epigenetic aging clock analysis using the **Biolearn** Python library on two publicly available GEO datasets.

### Datasets

| GEO ID | Study | Tissue | Samples | Platform |
|--------|-------|--------|---------|----------|
| GSE40279 | Hannum et al. 2013 | Whole blood | 656 | Illumina 450K |
| GSE87571 | Johansson et al. 2017 | Whole blood | 729 | Illumina 450K |

### Aging Clocks Applied

| Clock | Year | CpG Sites | Description |
|-------|------|-----------|-------------|
| Horvathv1 | 2013 | 353 | First pan-tissue epigenetic clock |
| Hannum | 2013 | 71 | Blood-specific age predictor |
| PhenoAge | 2018 | 513 | Phenotypic/biological age, mortality-linked |
| Lin | 2016 | 99 | Blood methylation age predictor |
| DunedinPACE | 2022 | 173 | Pace of biological aging (rate not age) |

### Tasks Completed

- **Task 1:** Loaded 2 complete GEO datasets via Biolearn DataLibrary
- **Task 2:** Successfully ran 5 aging clocks on both datasets
- **Task 3:** Described all datasets and clocks in detail
- **Task 4:** Generated Pearson correlation matrices across clocks (per dataset)
- **Task 5:** Generated age deviation heatmaps (predicted - chronological age)
- **Task 6:** Generated scatter plots of predicted vs chronological age

### Key Findings
- Traditional clocks (Horvathv1, Hannum, PhenoAge, Lin) highly correlated (r = 0.89-0.99)
- DunedinPACE lower correlation (r = 0.42-0.58) — measures pace not age
- Higher inter-clock correlations in GSE87571 vs GSE40279
- Consistent patterns validated across two independent datasets

---

## Key Results

### Part 1 - WGBS QC Summary
| Sample | Total Reads | %GC | Quality |
|--------|------------|-----|---------|
| subset_1 | 458,003 | 26.1% | PASS |
| subset_2 | 458,003 | 26.3% | PASS |

### Part 2 - Clock Performance (GSE40279)
| Clock | Pearson r | MAE (years) |
|-------|-----------|-------------|
| Horvathv1 | ~0.92 | ~5.2 |
| Hannum | ~0.93 | ~4.8 |
| PhenoAge | ~0.91 | ~6.1 |
| Lin | ~0.90 | ~5.5 |
| DunedinPACE | ~0.48 | N/A (pace units) |

---


## How Parts Connect
```
DNA Methylation
│
├── Changes in DISEASE (Part 1)
│     └── Cancer: aberrant methylation drives
│           tumor suppressor silencing
│
└── Changes with AGING (Part 2)
└── Aging clocks: methylation patterns
predict biological age
```
Both parts demonstrate that DNA methylation is a **versatile, stable, and informative epigenetic biomarker** applicable across biological contexts — from cancer diagnostics to aging research.
---

## Requirements

### Part 1 - Galaxy
- Galaxy Europe account: https://usegalaxy.eu
- Tutorial: https://training.galaxyproject.org/training-material/topics/epigenetics/tutorials/methylation-seq/tutorial.html

### Part 2 - Python
```bash
pip install biolearn pandas numpy matplotlib seaborn scipy
```

Or use Google Colab (recommended — no installation needed).

---

## How to Run Part 2

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/DNA-Methylation-Analysis.git
cd DNA-Methylation-Analysis/Part2_EPIC_Biolearn

# Install dependencies
pip install -r requirements.txt

# Run notebook
jupyter notebook notebooks/aging_clocks_analysis.ipynb
```

---

## References

1. **Lin et al. (2015).** Hierarchical Clustering of Breast Cancer Methylomes Revealed Differentially Methylated Long Non-coding RNAs. *PLOS ONE.*
2. **Ying et al. (2023).** Biolearn, an open-source library for biomarkers of aging. *bioRxiv.* https://doi.org/10.1101/2023.12.02.569722
3. **Hannum et al. (2013).** Genome-wide Methylation Profiles Reveal Quantitative Views of Human Aging Rates. *Molecular Cell.*
4. **Horvath (2013).** DNA methylation age of human tissues and cell types. *Genome Biology.*
5. **Levine et al. (2018).** An epigenetic biomarker of aging for lifespan and healthspan. *Aging.*
6. **Belsky et al. (2022).** DunedinPACE, a DNA methylation biomarker of the pace of aging. *eLife.*

---

## Authors
- **Abid Hussain** — Bioinformatics Student, NUST

## Acknowledgements
- Galaxy Training Network (GTN) for the WGBS tutorial
- Biolearn development team for the aging clocks library
- GEO database for public methylation datasets
