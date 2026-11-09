# H · AAV cassette annotation and WGS variant review

**Mode:** Coding agent · **Target:** one cassette size chart or one coordinate-join table.

## Public data

| Source | File or record | Use |
| --- | --- | --- |
| [McIntosh et al. and supplement](https://pmc.ncbi.nlm.nih.gov/articles/PMC3637010/) | Published shortened FVIII cassettes | Core cassette annotations |
| [RefSeq F8 NM_000132.4](https://www.ncbi.nlm.nih.gov/nuccore/NM_000132.4) and [RefSeq F9 NM_000133.4](https://www.ncbi.nlm.nih.gov/nuccore/NM_000133.4) | Versioned coding references | Sequence lengths and numbering |
| [FIX-Padua study](https://pubmed.ncbi.nlm.nih.gov/31219805/) and [Bryant et al. AAV2 data](https://github.com/alibashir/aav) | Published activity panels and capsid packaging assay results | Alternative protein and capsid analyses |
| [Peterson et al., Supplementary Data 1, 3 and 5](https://doi.org/10.1038/s42003-023-04974-0) | Candidate sites, variant calls and figure data | Core WGS comparison |
| [NCBI BioProject PRJNA687003](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA687003) and [EVA PRJEB61387](https://www.ebi.ac.uk/eva/?eva-study=PRJEB61387) | Raw WGS/CIRCLE-seq and primary-filtered VCFs | Archival and follow-up inputs |

## Choose one exercise

- **AAV/FVIII:** Annotate one published FVIII cassette. Plot the reported lengths of the ITRs, regulatory elements and coding sequence. Add a full-length F8 coding reference for scale, label it as a coding sequence, and mark elements with unresolved lengths. Extensions include comparing published FIX-Padua activity by FVIIIa condition or plotting the relationship between AAV2 sequence change and measured packaging performance.
- **Editing/WGS:** Select one published guide or founder. Join predicted sites with reported WGS variant calls in the paper's mm10 assembly. Keep the intended target, predicted off-target sites with a matched call, and sites with unresolved status in separate categories. Inspect the authors' comparison with untreated animals where available. Record the interval definition and VCF/BED coordinate conventions.

**Agent prompt**

> Show me the source rows, the units or coordinate system, and any missing fields. Then generate the selected chart or join table and manually verify one element or variant against the published record.
