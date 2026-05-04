# DNA Methylation Analysis — Project Report

## Introduction
DNA methylation is a fundamental epigenetic modification where a methyl group is added to cytosine residues, predominantly at CpG dinucleotides. It plays critical roles in gene regulation, development, and disease. This project investigates DNA methylation through two complementary approaches: cancer biology (WGBS) and aging biology (EPIC array aging clocks).

---

## Part 1: WGBS Analysis

### Background
Whole Genome Bisulfite Sequencing (WGBS) provides single-base resolution maps of DNA methylation across the entire genome. In bisulfite sequencing, unmethylated cytosines are converted to uracil (read as thymine), while methylated cytosines remain unchanged. This chemical conversion allows identification of methylated vs unmethylated CpG sites at single-nucleotide resolution.

### Dataset
Breast cancer methylation data from Lin et al. (2015), comparing tumor samples (BT089, BT126, BT198) with normal breast tissue (NB1, NB2). Data accessed via Zenodo DOI: 10.5281/zenodo.557099.

### Methods
1. **Quality Control (Falco):** Confirmed high read quality. Low %GC (~26%) is characteristic of bisulfite-converted DNA due to widespread C to T conversion.
2. **Alignment (bwameth):** Bisulfite-aware alignment to hg19 reference genome producing BAM files of ~60 MB per sample.
3. **Methylation Bias (MethylDackel mbias):** No significant position-dependent bias detected across all four strands. No read trimming was required.
4. **Methylation Extraction (MethylDackel extract):** CpG methylation levels extracted in bedGraph format covering chromosomes 1, 10, and 11.
5. **Visualization (computeMatrix + plotProfile):** Coverage enrichment confirmed at CpG island centers using ±500 bp reference-point mode.

### Results
- 458,003 high-quality reads per sample confirmed
- Successful alignment to hg19 reference genome
- No methylation bias detected — clean bisulfite conversion
- Coverage profile shows characteristic enrichment at CpG island centers
- Methylation levels extracted and available for downstream differential analysis

---

## Part 2: Aging Clocks Analysis

### Background
Epigenetic aging clocks are mathematical models that use DNA methylation patterns at specific CpG sites to estimate biological age. Different clocks capture different aspects of aging — some predict chronological age accurately, others predict mortality risk or the pace of aging. These clocks have become important tools in aging research and clinical applications.

### Datasets
- **GSE40279 (Hannum et al. 2013):** 656 whole blood samples, ages 19-101, Illumina 450K array. This is the dataset used to originally build the Hannum aging clock.
- **GSE87571 (Johansson et al. 2017):** 729 whole blood samples across a wide age range, Illumina 450K array. Used for population-level aging methylation studies.

### Aging Clocks Applied
Five clocks were applied to both datasets: Horvathv1, Hannum, PhenoAge, Lin, and DunedinPACE.

### Results

#### Task 4: Correlation Matrix
Traditional clocks (Horvathv1, Hannum, PhenoAge, Lin) show very high mutual Pearson correlations (r = 0.89-0.99), confirming they capture a similar underlying biological aging signal driven by the same methylation changes. DunedinPACE shows lower correlations with other clocks (r = 0.42-0.58) because it measures the pace (rate) of aging rather than biological age itself — making it a fundamentally different biological construct.

GSE87571 shows even higher inter-clock correlations (r = 0.99 for most pairs) compared to GSE40279, suggesting more homogeneous aging patterns in that cohort.

#### Task 5: Age Deviation Heatmap
- Most traditional clocks show balanced deviations (positive and negative) across samples
- DunedinPACE consistently shows large negative deviations — expected since DunedinPACE outputs values around 1.0 (pace units per year), not years of age
- Samples sorted by chronological age reveal gradual deviation patterns in some clocks

#### Task 6: Scatter Plots
- Traditional clocks show strong linear relationships with chronological age (r = 0.90-0.99)
- DunedinPACE shows weaker correlation as it measures a different biological construct
- All traditional clocks show statistically significant predictions (p < 0.001)

---

## Discussion

### Comparison of Two Approaches
WGBS and EPIC array methylation analysis serve complementary purposes:
- WGBS provides genome-wide, single-base resolution — ideal for discovering novel methylation changes in cancer
- EPIC arrays profile pre-selected CpG sites efficiently across large cohorts — ideal for aging clock applications

### Biological Significance
The high concordance between traditional aging clocks (r > 0.89) confirms that DNA methylation changes with age in a highly reproducible and consistent manner across individuals. This robustness is what makes epigenetic clocks useful as biomarkers.

DunedinPACE's distinct behavior highlights an important distinction: measuring biological age at a single timepoint vs measuring the rate of aging over time provide complementary but different information about an individual's aging trajectory.

---

## Conclusion
This project demonstrates that DNA methylation is a versatile biomarker applicable to both disease (cancer) and normal biology (aging). WGBS provides genome-wide resolution suitable for detecting cancer-specific methylation changes, while EPIC array data enables efficient aging clock analysis across large cohorts. The high concordance between traditional aging clocks confirms the robustness of the epigenetic aging signal, while DunedinPACE highlights the complementary information provided by pace-of-aging measures.

---

## References
1. Lin et al. (2015). Hierarchical Clustering of Breast Cancer Methylomes. PLOS ONE.
2. Ying et al. (2023). Biolearn, an open-source library for biomarkers of aging. bioRxiv.
3. Hannum et al. (2013). Genome-wide Methylation Profiles Reveal Quantitative Views of Human Aging Rates. Molecular Cell.
4. Horvath (2013). DNA methylation age of human tissues and cell types. Genome Biology.
5. Levine et al. (2018). An epigenetic biomarker of aging for lifespan and healthspan. Aging.
6. Lu et al. (2019). DNA methylation GrimAge strongly predicts lifespan and healthspan. Aging.
7. Belsky et al. (2022). DunedinPACE, a DNA methylation biomarker of the pace of aging. eLife.
