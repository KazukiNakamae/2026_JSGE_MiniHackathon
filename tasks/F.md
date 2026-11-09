# F · Cre recognition specificity and model transfer

**Mode:** Coding agent · **Target:** a focused recreation of a recognition profile.

## Question

At which recognition positions do wild-type Cre and Cre R259A differ? Recreate the enrichment profile in [Bessen et al., Figure 2b](https://doi.org/10.1038/s41467-019-09987-0) from the published Rec-seq values.

## Public data

| Source | File or record | Use |
| --- | --- | --- |
| [Bessen et al. Source Data](https://www.nature.com/articles/s41467-019-09987-0) | Source Data for Figure 2b | Core enrichment profiles |
| [NCBI BioProject PRJNA517947](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA517947) | Rec-seq reads | Original sequencing archive |
| [Zebrafish comparison, Supplementary Table 3](https://doi.org/10.1186/s12864-021-08238-1) and [ENA PRJEB39643](https://www.ebi.ac.uk/ena/browser/view/PRJEB39643) | Candidate-count summaries and underlying CIRCLE-seq reads | Alternative to examine cross-species transfer |

## Core exercise

1. Locate the Figure 2b values in the source file. Record position labels, direction, and enrichment units.
2. Plot both profiles on the same axes and mark the position with the largest observed difference in the plotted range. Trace two plotted values to the source.
3. Report what the enrichment profile says about recognition at those positions. Record any separate activity measurements as a different endpoint.

**Agent prompt**

> Inspect the Figure 2b source sheet, convert it to a tidy table, recreate the two lines, and show the original cells for the two positions you highlight.

**Transfer alternative:** Reproduce the per-guide CRISPRScan versus CIRCLE-seq count plot in zebrafish Figure 3A. The summary counts support a coverage comparison. To evaluate transfer or fine-tuning, prepare per-site labels from a comparable assay and hold out entire guides. Start with the frozen baseline in [B](B.md) and compare any recalibration on the same held-out guides.
