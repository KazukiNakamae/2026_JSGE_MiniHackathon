# E · Single-cell genotype and protein phenotype

**Mode:** Browser and spreadsheet · **Target:** one group summary and a short interpretation of a published panel.

## Question

How can a DNA variant and a surface-protein phenotype be compared in the same cells? Use Figure 3b of [Demaree et al.](https://www.nature.com/articles/s41467-021-21810-3), which overlays NPM1-mutant cells on the CD33 and CD34 signals for all displayed cells.

## Public data

| Source | Record | Use |
| --- | --- | --- |
| [Demaree et al., Figure 3 and Source Data](https://www.nature.com/articles/s41467-021-21810-3) | Published panels and accompanying source file | Browser exercise and source inspection |
| [NCBI BioProject PRJNA602320](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA602320) | Original DAb-seq reads | Study provenance |
| [UniVI processed data catalog](https://univi.readthedocs.io/en/latest/reproducibility/datasets.html#aml-mosaic) | Zenodo record 22883601, `aml_mosaic_dabseq_adt.h5ad` | Optional instructor export of paired genotype and antibody values |

## Core exercise

1. Open Figure 3b and select one time point. Label the axes and identify the two overlays. The all-cell background includes the highlighted cells. Describe one pattern visible in the plot.
2. Paste the synthetic example below into a spreadsheet. For each genotype call, calculate the fraction of cells with high CD34. Keep the no-call category separate. Make a bar chart and write the denominator for each bar.
3. For a data-based extension, use an instructor-exported CSV from one processed DAb-seq sample with cell ID, sample ID, NPM1 call, CD33, and CD34. Filter to that sample, keep failed genotype calls separate, and compare marker distributions or use an instructor-specified marker threshold.

### Synthetic practice counts

These counts illustrate the spreadsheet operation and are independent of the paper.

| NPM1 call | CD34 high | CD34 low |
| --- | ---: | ---: |
| Variant detected | 3 | 7 |
| Reference call | 6 | 4 |
| No call | 1 | 3 |

Bring the chart and one sentence about what the DNA assay measured. DAb-seq reads a targeted panel of genomic loci and measures surface proteins for the same cells.
