# Part 1: WGBS DNA Methylation Analysis (Galaxy)

## Overview
Whole Genome Bisulfite Sequencing (WGBS) analysis of breast cancer methylation data using the Galaxy bioinformatics platform, following the Galaxy Training Network (GTN) tutorial.

**Reference:** Lin et al. (2015). Hierarchical Clustering of Breast Cancer Methylomes Revealed Differentially Methylated Long Non-coding RNAs. PLOS ONE.

---

## Platform
- **Galaxy Europe:** https://usegalaxy.eu
- **History Name:** DNA-Methylation-WGBS
- **Tutorial:** https://training.galaxyproject.org/training-material/topics/epigenetics/tutorials/methylation-seq/tutorial.html

---

## Data
- **Source:** Zenodo DOI: 10.5281/zenodo.557099
- **Input files:** subset_1.fastq, subset_2.fastq, aligned_subset.bam
- **Reference genome:** hg19 (GRCh37)
- **Samples:**
  - Tumor: BT089, BT126, BT198
  - Normal: NB1, NB2

---

## Pipeline Steps

### Step 1: Data Upload
- Loaded files from GTN Shared Data Library
- Path: GTN-Material → Epigenetics → DNA Methylation data analysis → DOI: 10.5281/zenodo.557099

### Step 2: Quality Control — Falco
- **Tool:** Falco v1.3.0+galaxy0
- **Bisulfite mode:** Enabled
- **Results:**

| Sample | Total Sequences | %GC | Per Base Quality | Per Sequence Quality |
|--------|----------------|-----|-----------------|---------------------|
| subset_1 | 458,003 | 26.1% | PASS | PASS |
| subset_2 | 458,003 | 26.3% | PASS | PASS |

- Low %GC is expected for bisulfite data due to C→T conversion

### Step 3: Alignment — bwameth
- **Tool:** bwameth
- **Reference:** hg19 built-in index
- **Mode:** Single-end
- **Output:** BAM alignment files (~60 MB each)

### Step 4: Methylation Bias — MethylDackel (mbias)
- **Tool:** MethylDackel v0.5.2+galaxy0
- **Mode:** Determine position-dependent methylation bias
- **Reference:** hg19
- **Output:** SVG bias plots for all 4 strands (OT, OB, CTOT, CTOB)
- **Conclusion:** No significant bias detected, no trimming required

### Step 5: Methylation Extraction — MethylDackel (extract)
- **Tool:** MethylDackel v0.5.2+galaxy0
- **Mode:** Extract methylation metrics
- **Input:** aligned_subset.bam (precomputed)
- **Reference:** hg19
- **Output:** CpG methylation levels in bedGraph format
- **Coverage:** chr1, chr10, chr11

### Step 6: bamCoverage
- **Tool:** bamCoverage v3.5.4
- **Input:** aligned_subset.bam
- **Normalization:** 1x (RPGC)
- **Genome size:** hg19 (2,685,511,504)
- **Output:** bigwig file (4.7 MB)

### Step 7: computeMatrix
- **Tool:** computeMatrix v3.5.4
- **Regions:** CpGIslands.bed
- **Score file:** bamCoverage bigwig
- **Mode:** reference-point
- **Reference point:** center of region
- **Distance:** 500 bp upstream and downstream

### Step 8: plotProfile
- **Tool:** plotProfile v3.5.4
- **Input:** computeMatrix output
- **Output:** Coverage profile PNG
- **Result:** Clear enrichment peak at CpG island centers

---

## Results Files

| File | Description |
|------|-------------|
| `results/falco_reports/falco_subset1_basic_stats.png` | QC basic statistics - subset 1 |
| `results/falco_reports/falco_subset2_basic_stats.png` | QC basic statistics - subset 2 |
| `results/falco_reports/falco_subset1_quality_scores.png` | Quality score distribution - subset 1 |
| `results/falco_reports/falco_subset2_quality_scores.png` | Quality score distribution - subset 2 |
| `results/visualization/plotProfile_CpG_islands.png` | Coverage profile around CpG islands |
| `results/visualization/methylation_heatmap_data.tabular` | Heatmap data tabular |
| `results/methylation_extraction/methylation_levels.tabular` | CpG methylation levels |

---

## Key Findings
- Bisulfite conversion confirmed by low %GC (~26%)
- High quality reads with tight Phred score distribution (peak at Q35+)
- Coverage enrichment visible at CpG island centers in plotProfile
- Methylation levels successfully extracted for chr1, chr10, chr11
