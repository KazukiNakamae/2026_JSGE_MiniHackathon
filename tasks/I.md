# I · Guide RNA structure and Cas9 activity

**Mode:** Coding agent · **Target:** one structure–activity comparison table or one Cas9 activity plot.

## Public data

| Source | File or record | Use |
| --- | --- | --- |
| [Bush et al. BLADE article and supplementary data](https://pmc.ncbi.nlm.nih.gov/articles/PMC10529641/) | Reported scaffold sequences and GFP-knockout results, including the Data S2 reference | Core guide scaffold comparison |
| [ViennaRNA RNAfold documentation](https://www.tbi.univie.ac.at/RNA/RNAfold.1.html) | RNA secondary-structure predictor | Fold identical-spacer guide pairs with recorded settings |
| [Spencer and Zhang, Cas9 deep mutational scanning](https://doi.org/10.1038/s41598-017-17081-y) | Published single-mutant activity panels | Cas9 alternative using matched and mismatched targets |

## Choose one exercise

- **Guide scaffold:** Select a wild-type scaffold and two reported variants tested with the same spacer and assay conditions. Fold each complete guide with RNAfold. Compare the predicted structures and energies alongside the published GFP-knockout readout. Include the assay, target, and source panel for each row.
- **Cas9 protein:** Select wild type and a few reported single substitutions tested with the same target pair. Plot activity on the matched target against activity on the mismatched target, using the reported units and assay conditions. If the numbers are read from a figure, label them as digitized and record the panel.
- **Spacer extension:** Use [B](B.md) to assess the effect of spacer–target mismatches with the scaffold held constant.

**Agent prompt**

> Verify that the variants share the stated spacer or target pair and assay context. Build a source-linked table, run the selected computation, and label measured and predicted quantities separately.
