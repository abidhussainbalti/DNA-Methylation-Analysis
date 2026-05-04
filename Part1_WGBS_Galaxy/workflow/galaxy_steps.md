# Galaxy Workflow Steps — WGBS DNA Methylation Analysis

## Platform
- Galaxy Europe: https://usegalaxy.eu
- History Name: DNA-Methylation-WGBS

---

## Step-by-Step Execution

### 1. Create History
- Click "+" in history panel (top right)
- Rename to: DNA-Methylation-WGBS

### 2. Upload Data
- Shared Data → Data Libraries → GTN-Material → Epigenetics
- → DNA Methylation data analysis → DOI: 10.5281/zenodo.557099
- Selected files:
  - subset_1.fastq
  - subset_2.fastq
  - aligned_subset.bam
  - CpGIslands.bed
- Click "Add to History" → "Add as Datasets"

### 3. Falco QC
- Tool: Falco (v1.3.0+galaxy0)
- Input: subset_1.fastq AND subset_2.fastq (batch mode)
- Bisulfite Sequencing: YES
- Output: RawData + Webpage for each file (4 datasets total)

### 4. bwameth Alignment
- Tool: bwameth
- Reference: Built-in hg19
- Single-end mode
- Input: subset_1.fastq and subset_2.fastq
- Output: 2 BAM files (~60 MB each)

### 5. MethylDackel Bias Analysis
- Tool: MethylDackel
- Mode: Determine position-dependent methylation bias (mbias)
- Reference: hg19
- Input: bwameth BAM outputs (both)
- Output: 8 SVG bias plots (4 strands x 2 samples)
- Result: No trimming needed

### 6. MethylDackel Extraction
- Tool: MethylDackel
- Mode: Extract methylation metrics (extract)
- Reference: hg19
- Input: aligned_subset.bam
- Output: CpG methylation levels bedGraph

### 7. bamCoverage
- Tool: bamCoverage
- Input: aligned_subset.bam
- Genome size: hg19/GRCh37
- Output format: bigwig
- Normalization: 1x (RPGC)
- Bin size: 50

### 8. computeMatrix
- Tool: computeMatrix
- Regions (BED): CpGIslands.bed
- Score file: bamCoverage bigwig output
- Mode: reference-point
- Reference point: center of region
- Distance upstream: 500
- Distance downstream: 500

### 9. plotProfile
- Tool: plotProfile
- Input: computeMatrix output
- Output: PNG coverage profile plot

---

## Tools Summary

| Tool | Version | Purpose |
|------|---------|---------|
| Falco | 1.3.0+galaxy0 | Quality control |
| bwameth | - | Bisulfite-aware alignment to hg19 |
| MethylDackel | 0.5.2+galaxy0 | Methylation bias and extraction |
| bamCoverage | 3.5.4+galaxy0 | BAM to bigwig conversion |
| computeMatrix | 3.5.4+galaxy0 | Matrix for visualization |
| plotProfile | 3.5.4+galaxy0 | Coverage profile plot |
| bedtools sort | - | Sorting bedGraph files |

---

## Notes
- Always use hg19 (not hg38) for all tools — data was aligned to hg19
- Bisulfite mode must be enabled in Falco
- Use aligned_subset.bam from library for extraction steps
