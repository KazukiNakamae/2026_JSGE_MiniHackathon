# G · Tet-On expression over time

**Mode:** Coding agent · **Target:** separate plots for induced response and basal expression.

## Question

How do the published Tet-On conditions compare for expression after 30 days and for expression without doxycycline? Replot the relevant panels in [Yanagi et al.](https://doi.org/10.1186/s13036-025-00499-8).

## Public data

| Source | File or record | Use |
| --- | --- | --- |
| [Yanagi et al. and supplemental source data](https://link.springer.com/article/10.1186/s13036-025-00499-8) | Figure 2H and Figure 4A source data | Late inducibility and basal expression in their respective comparisons |
| [Published rtTA variant study](https://doi.org/10.1016/S0014-5793%2802%2902587-5) | Reported dose-response panels | Alternative if the focus is the transactivator |
| [Meir et al., Figure 5](https://pmc.ncbi.nlm.nih.gov/articles/PMC3078864/) and [Tol2 genomic insertion supplement](https://doi.org/10.1186/1471-2164-10-418) | piggyBac and Tol2 reference comparison, plus Tol2 flanking sequences and target-site counts | Transposon alternative |

## Core exercise

1. Extract the reported day-30 induced-response values and the basal-expression values for the stated groups. Record doxycycline status, condition, time point, unit, replicate, and figure panel.
2. Plot the two comparisons separately with their original units and controls. Calculate within-panel differences or ratios only where the underlying values support them.
3. State whether the same groups were compared across panels and identify which measurement addresses expression persistence and which addresses leak.

**Agent prompt**

> Read the paper and source data for Figures 2H and 4A. Show the condition and unit mapping first. Then produce two labeled plots and link each value to the correct panel.

**Alternatives:** Compare published rtTA dose-response curves, or recreate a Tol2 motif logo from the public insertion supplement and compare it with the published piggyBac TTAA pattern. Record the source, organism, and background for each motif. [Evo 2](https://doi.org/10.1038/s41586-026-10176-5) provides a separate chromatin-design example. Use the Tet-On measurements to evaluate persistence and leak in this exercise.
