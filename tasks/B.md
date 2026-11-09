# B · Off-target candidate coverage and ranking

**Mode:** Coding agent · **Target:** a per-guide coverage and top-20 recovery table.

## Question

How many observed sites does each search recover, and how much does scoring change the rank of those sites? Adapt the candidate comparison in [Haeussler et al., Supplementary Table 2](https://doi.org/10.1186/S13059-016-1012-2).

## Public data

| Source | File or record | Use |
| --- | --- | --- |
| [CRISPOR paper repository](https://github.com/maximilianh/crisporPaper) | `offtargets.tsv` and the README entry for `compareOfftargetTools.py` | Curated assay detections and original comparison inputs |
| [NCBI GEO GSE61586](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE61586) | Tsai et al. GUIDE-seq records | Provenance for the selected GUIDE-seq detections |
| [UCSC hg19 reference](https://hgdownload.soe.ucsc.edu/goldenPath/hg19/bigZips/) | Reference sequence for the historical comparison | Coordinate and sequence checks |
| [MOFF](https://github.com/MDhewei/MOFF) | Published score implementation | Rank ungapped guide–target pairs |

## Core exercise

1. Select two Tsai GUIDE-seq guides with curated detections. Prefer the author's tool-output tables if available. If rerunning the tools, use the same assembly, PAM, mismatch limit and bulge policy for both tools, then record versions and settings.
2. Match by guide, chromosome, strand and a consistently defined site coordinate. Spot-check a match by sequence. Report eligible observed sites, recovered sites per tool, and any observed sites excluded by the common search rules.
3. Rank the same ungapped candidate set by mismatch count, CFD, and MOFF. Record the tie rule, score direction, candidate count, and how many observed sites appear in the first 20 for each ranking.

**Agent prompt**

> Audit the guide IDs, assembly, coordinate convention, and scoring columns. Write a join I can inspect and a per-guide table that separates enumeration coverage from ranking. Print one matched and one unmatched example with their source rows.

**Extension:** If a dataset has independently assayed sites and suitable negative labels, fit a simple baseline and split train and evaluation sets by guide sequence. Treat GUIDE-seq non-detections as unlabeled and check whether the published model trained on the evaluation guides.
