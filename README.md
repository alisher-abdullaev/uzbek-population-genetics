# uzbek-population-genetics
CODE ARCHIVE SUMMARY. Generated as a Supplementary file for the Methods section in manuscript: "Genetic architecture of the Uzbek population: a systematic  review and quantitative analysis of population differentiation at clinically  relevant loci"
# Uzbek Population Genetics Analysis

**Complete analysis code for:** "Genetic architecture of the Uzbek population: a systematic review and quantitative analysis of population differentiation at clinically relevant loci"

Authors: Alisher Abdullaev¹, Anastasiya Punko¹, Yuliya Kapralova¹, Darya Zakirova¹, Guzal Abdullaeva², Inga Prokopenko³⁴, Dilbar Dalimova¹, Shahlo Turdikulova¹

¹Center for Advanced Technologies, Ministry of Higher Education, Science and Innovation, Tashkent, Uzbekistan  
²Republican Specialized Cardiological Scientific and Practical Medical Center, Tashkent, Uzbekistan  
³Department of Clinical Medicine, University of Surrey, Guildford, United Kingdom  
⁴People-Centred Artificial Intelligence Institute, University of Surrey, United Kingdom

---

## Table of Contents

- [Overview](#overview)
- [Installation](#installation)
- [Usage](#usage)
- [Analysis Pipeline](#analysis-pipeline)
- [Output Files](#output-files)
- [Dependencies](#dependencies)
- [Citation](#citation)
- [License](#license)

---

## Overview

This repository contains all statistical analysis code used in our study of population genetic structure in the Uzbek population. The analysis examines 54 pharmacogenetic and disease-associated variants across 230 Uzbek individuals compared to European (EUR) and East Asian (EAS) reference populations from the 1000 Genomes Project.

**Key Analyses:**
- Wright's F<sub>ST</sub> with bootstrap confidence intervals (Weir & Cockerham 1984)
- Principal Component Analysis (PCA)
- Two-way admixture estimation (K=2)
- Comprehensive statistical tests (χ², Fisher's Exact, G-test, Cochran-Armitage, Mantel)
- Publication-quality visualizations at 300 dpi

**Main Findings:**
- Uzbeks show intermediate positioning: 65.3% West Eurasian, 34.7% East Asian ancestry
- Closer genetic affinity to Europeans (F<sub>ST</sub> = 0.018) than East Asians (F<sub>ST</sub> = 0.045)
- 47/54 loci significant after Bonferroni correction (p < 9.26×10⁻⁴)
- Strong EUR-Uzbek correlation (r² = 0.76) suggests ~76% PRS transferability

---

## Installation

### Requirements

- Python 3.12
- pip package manager

### Setup

```bash
# Clone the repository
git clone ßhttps://github.com/alisher-abdullaev/uzbek-population-genetics.git
cd uzbek-population-genetics

# Create virtual environment (recommended)
python3.12 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### Dependencies

All exact versions specified in `requirements.txt`:

```
numpy==1.26.4
pandas==2.1.4
scipy==1.11.4
scikit-learn==1.3.2
matplotlib==3.8.3
seaborn==0.13.2
```

---

## Usage

### Input Data Format

Place your allele frequency data in `data/allele_frequencies.csv` with the following format:

```csv
Gene,Uzbek_RAF,EUR_RAF,EAS_RAF
MTHFR,0.31,0.36,0.30
F5,0.21,0.01,0.00
AGT,0.60,0.58,0.14
...
```

**Column descriptions:**
- `Gene`: Gene symbol (duplicates allowed with suffixes like `AGT_2`)
- `Uzbek_RAF`: Risk/reference allele frequency in Uzbek population
- `EUR_RAF`: Frequency in Europeans (1000 Genomes EUR super-population)
- `EAS_RAF`: Frequency in East Asians (1000 Genomes EAS super-population)

### Running the Analysis

Execute scripts in order:

```bash
# 1. FST calculation with bootstrap CIs
python scripts/01_fst_analysis.py

# 2. Principal Component Analysis
python scripts/02_pca_analysis.py

# 3. Admixture estimation
python scripts/03_admixture_analysis.py

# 4. Comprehensive statistical tests
python scripts/04_statistical_tests.py

# 5. Generate all visualizations
python scripts/05_visualizations.py
```

Or run all at once:

```bash
bash run_all.sh  # Unix/Mac
run_all.bat      # Windows
```

---

## Analysis Pipeline

### 1. F<sub>ST</sub> Analysis (`01_fst_analysis.py`)

Implements Weir & Cockerham (1984) F<sub>ST</sub> estimator with 10,000 bootstrap iterations for confidence intervals.

**Output:**
- `fst_results.csv`: Per-locus F<sub>ST</sub> values with 95% CIs
- `fst_matrix.csv`: Pairwise population F<sub>ST</sub> matrix
- `nei_matrix.csv`: Nei's genetic distance matrix

**Reference:**
> Weir BS, Cockerham CC. Estimating F-statistics for the analysis of population structure. *Evolution*. 1984;38(6):1358-1370.

### 2. PCA (`02_pca_analysis.py`)

Performs PCA on both populations (3×54 matrix) and loci (54×3 matrix) to identify major axes of variation.

**Output:**
- `pca_coordinates.csv`: Population coordinates on PC1-PC2
- `loci_pca_coordinates.csv`: Loci positions
- `pca_gene_loadings.csv`: Gene contributions to each PC (sorted by importance)

### 3. Admixture Analysis (`03_admixture_analysis.py`)

Maximum likelihood estimation of West Eurasian vs East Asian ancestry using ancestry-informative markers (AIMs).

**Selection criteria for AIMs:**
- F<sub>ST</sub>(EUR-EAS) > 0.25
- Qualified AIMs: 12 loci

**Output:**
- `admixture_individuals.csv`: Individual-level ancestry proportions (n=230)
- `admixture_regional.csv`: Regional summary statistics
- `ancestry_informative_markers.csv`: List of 12 AIMs

**Reference:**
> Alexander DH, Novembre J, Lange K. Fast model-based estimation of ancestry in unrelated individuals. *Genome Res*. 2009;19(9):1655-1664.

### 4. Statistical Tests (`04_statistical_tests.py`)

Comprehensive battery of tests for population differentiation:

**Tests performed:**
1. **Chi-square** (3 populations): Tests for allele frequency differences
2. **Fisher's Exact** (pairwise): Exact test for 2×2 contingency tables
3. **G-test**: Likelihood ratio alternative to chi-square
4. **Cochran-Armitage**: Tests for monotonic trends along Uzbek→EUR→EAS axis
5. **Mantel test**: Correlation between genetic (F<sub>ST</sub>) and geographic distance

**Multiple testing correction:** Bonferroni (α = 0.05/54 = 9.26×10⁻⁴)

**Output:**
- `chi_square_results.csv`
- `fisher_exact_results.csv`
- `gtest_results.csv`
- `cochran_armitage_results.csv`
- `mantel_results.csv`

### 5. Visualizations (`05_visualizations.py`)

Generates all manuscript figures at 300 dpi, publication-ready.

**Figures generated:**
- `Figure2_FST_Heatmap.png`: Pairwise F<sub>ST</sub> matrix + UPGMA dendrogram
- `Figure3_PCA_Biplot.png`: Population clustering with gene loadings
- `Figure4_FST_Manhattan.png`: Locus-specific differentiation
- `Figure6_Admixture_Barplot.png`: Individual ancestry proportions (n=230)
- `SuppFigure_Statistical_Summary.png`: Comprehensive test results

---

## Output Files

All results saved to `outputs/` directory:

### CSV Files (Data)
```
fst_results.csv                  - Per-locus FST with CIs
fst_matrix.csv                   - Population FST matrix
nei_matrix.csv                   - Nei's genetic distances
pca_coordinates.csv              - Population PCA coords
loci_pca_coordinates.csv         - Loci PCA coords
pca_gene_loadings.csv            - PC loadings by gene
admixture_individuals.csv        - Individual ancestry (n=230)
admixture_regional.csv           - Regional summary
ancestry_informative_markers.csv - 12 AIMs
chi_square_results.csv           - χ² test results
fisher_exact_results.csv         - Fisher's Exact results
gtest_results.csv                - G-test results
cochran_armitage_results.csv     - Trend test results
mantel_results.csv               - IBD test results
```

### PNG Files (Figures, 300 dpi)
```
Figure2_FST_Heatmap.png
Figure3_PCA_Biplot.png
Figure4_FST_Manhattan.png
Figure6_Admixture_Barplot.png
SuppFigure_Statistical_Summary.png
```

---

## Sample Sizes

- **Uzbek**: n = 230 (aggregated from 32 published studies)
- **EUR**: n = 1,000 (1000 Genomes Project Phase 3)
- **EAS**: n = 1,000 (1000 Genomes Project Phase 3)

---

## Key Results Summary

| Comparison | F<sub>ST</sub> | 95% CI | Nei's D |
|------------|---------|---------|---------|
| Uzbek vs EUR | 0.018 | 0.010-0.029 | 0.024 |
| Uzbek vs EAS | 0.045 | 0.023-0.080 | 0.061 |
| EUR vs EAS | 0.075 | 0.047-0.106 | 0.090 |

**Admixture proportions:**
- West Eurasian: 65.3% (95% CI: 61.2-69.1%)
- East Asian: 34.7% (95% CI: 30.9-38.8%)

**Correlations (allele frequencies):**
- Uzbek-EUR: r = 0.871, r² = 0.76, p = 1.22×10⁻¹⁷
- Uzbek-EAS: r = 0.632, r² = 0.40, p = 3.02×10⁻⁷
- EUR-EAS: r = 0.542, r² = 0.29, p = 2.32×10⁻⁵

---

## Troubleshooting

**Q: Script fails with "Input file not found"**  
A: Ensure `data/allele_frequencies.csv` exists with proper column names

**Q: Figures don't generate**  
A: Run scripts 01-04 first to create required input data

**Q: Different results on re-run**  
A: Random seed is set (42) for reproducibility. Results should be identical.

**Q: Memory error**  
A: Bootstrap with 10,000 iterations is memory-intensive. Reduce `N_BOOTSTRAP` in script if needed.

---

## Citation

If you use this code, please cite:

```bibtex
@article{abdullaev2025uzbek,
  title={Genetic architecture of the Uzbek population: a systematic review and quantitative analysis of population differentiation at clinically relevant loci},
  author={Abdullaev, Alisher and Punko, Anastasiya and Kapralova, Yuliya and Zakirova, Darya and Abdullaeva, Guzal and Prokopenko, Inga and Dalimova, Dilbar and Turdikulova, Shahlo},
  journal={[Journal Name]},
  year={2025},
  doi={[DOI]}
}

@software{abdullaev2025code,
  author={Abdullaev, Alisher},
  title={uzbek-population-genetics: Analysis code},
  year={2025},
  doi={10.5281/zenodo.XXXXXXX},
  url={https://github.com/https://github.com/alisher-abdullaev/uzbek-population-genetics.git}
}
```

---

## License

MIT License - see [LICENSE](LICENSE) file for details.

---

## Contact

**Corresponding Authors:**
- Alisher Abdullaev: [email]
- Shahlo Turdikulova: [email]

**Issues:** https://github.com/https://github.com/alisher-abdullaev/uzbek-population-genetics.git

---

## Acknowledgments

- 1000 Genomes Project Consortium for reference population data
- Ministry of Higher Education, Science and Innovation of Uzbekistan for funding
- All participants in the included studies

---

**Last Updated:** March 2025
