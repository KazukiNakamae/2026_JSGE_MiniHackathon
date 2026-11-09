# A · Gene expression in a mouse disease model

**Mode:** Coding agent · **Target:** one normalized expression plot and a small differential-expression table.

## Question

Which genes change between 5xFAD and wild-type mice in one matched hippocampus comparison? Reanalyze the count matrix from [Forner et al.](https://www.nature.com/articles/s41597-021-01054-y) using one age, sex, and brain region.

## Public data

| Source | File or record | Use |
| --- | --- | --- |
| [NCBI GEO GSE168137](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE168137) | `GSE168137_countList.txt.gz` and GEO sample annotations | Core count matrix and group assignments |
| [NCBI GEO GSE143758](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE143758) | Habib et al. hippocampal single-nucleus RNA-seq | Extension using annotated astrocyte nuclei |

## Core exercise

1. Select eight-month-old female hippocampus samples. Match count columns to GEO metadata and record the actual sample count per genotype. Resolve mismatches before analysis.
2. Use integer counts with DESeq2 and the design `~ genotype`. Record the direction as 5xFAD over wild type and keep one biological replicate per mouse for this tissue comparison.
3. Plot normalized expression for a few genes with individual mice visible. Save a table with gene ID, log2 fold change, adjusted p value, and sample counts. Trace one plotted value and one sample label to the input.

**Agent prompt**

> Inspect the count table and GEO sample metadata. Show me the selected sample IDs and genotype assignments before fitting the model. Then write one reproducible R script that produces the plot and table for the matched hippocampus comparison.

## Extensions

- For single-nucleus analysis, use an annotated subset of GSE143758 and plot the fraction of disease-associated astrocytes per mouse. For a spatial alternative, use the public mouse-brain dataset in [D](D.md).
- For guide design, inspect one public mouse locus in [BE-Designer](https://doi.org/10.1186/s12859-018-2585-4). Record the PAM, editing window, and possible bystander bases, then use the candidate-search workflow in [B](B.md).
