# C · AAV integration sites and counting bias

**Mode:** Coding agent · **Target:** an integration-count plot and a small counting-bias example.

## Question

How does the interpretation change when the unit is an integration locus, a read, or an input genome? Use the reported unique integration loci (UILs) in the [ITR-seq study](https://pmc.ncbi.nlm.nih.gov/articles/PMC10659022/).

## Public data

| Source | File or record | Use |
| --- | --- | --- |
| [ITR-seq study and public supplements](https://pmc.ncbi.nlm.nih.gov/articles/PMC10659022/) | Table 1, Figure 1A and `Supp_DataS1.xlsx` | Published counts and, when the fields are available, a sample-level or locus-level extension |
| [Penn-GTP ITR-seq pipeline](https://github.com/Penn-GTP/ITR_Seq2_public) | ITR-seq v2 public repository | Definitions and upstream pipeline |
| [ProteomeXchange PXD044049](https://proteomecentral.proteomexchange.org/cgi/GetDataset?ID=PXD044049) and [Synapse syn52256996](https://www.synapse.org/Synapse:syn52256996) | EGF phosphoproteomics archive and processed MCF10A time series | Alternative for phosphoproteomics |

## Core exercise

1. Extract the recombinant AAV and wild-type AAV macaque summaries from Table 1. Make a dot plot of the group means for unique loci per 100 input genomes reported with Figure 1A. Keep the original groups, units, and sample denominators. If the supplement provides sample-level values, make a compact version of Figure 1A. If chromosome and locus are available, add a per-chromosome count for one cohort.
2. Make a clearly labeled synthetic table with equal input molecule counts at two loci. Add excess duplicate reads at one locus and missed recovery at the other in separate scenarios. Compare counts by read, recovered molecule, and unique site.
3. Save the public-data plot and synthetic example separately. Report which denominator is documented for each output.

**Agent prompt**

> Inspect the public table and explain every counting unit. Extract the reported values with a source column, plot them, and create a separate synthetic example that illustrates duplicate reads and missing recovery.

**Alternatives:** Use [B](B.md) for nuclease off-target data. For phosphoproteomics, plot three to five measured EGFR–MAPK phosphosites over time from the processed EGF table and preserve missing values.
