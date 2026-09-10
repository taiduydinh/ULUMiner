# JPER v3 public reproducibility notes

Updated: 2026-09-10

## Current public repository layout

The public notebook uses these names:

- `inputs/`
- `neqFin_outputs/`
- `JPER_outputs_v3/`
- `cities/`
- `JPER_v3.ipynb`
- `JPER_v3_notes.md`
- `negFIN.py`
- `requirements_v3.txt`
- `summary_all_cities.xlsx`

A fresh run writes only to `JPER_reproduced_outputs_v3/`.

## 2026-09-10 negFIN integration update

The useful beginning of the earlier `Euro_analysis.ipynb` showed how `negFIN.py` was run for each transaction dataset and how the all-city support matrix was assembled. That functionality has now been integrated into `JPER_v3.ipynb`, but adapted to the current repository layout and the corrected v3 analysis.

The new public workflow:

1. reads the 100 transaction files directly from `inputs/`;
2. loads a fresh copy of `negFIN.py` for each city, preventing module-level state from carrying between datasets;
3. runs negFIN at the primary relative support threshold `0.10`;
4. writes fresh results to `JPER_reproduced_outputs_v3/negFIN_outputs_10pct/`;
5. reconstructs `summary_all_cities_from_negFIN_10pct.xlsx` from negFIN's rounded `%:` field;
6. reconstructs an exact support matrix from negFIN's `#SUP:` integer counts;
7. independently re-mines the transactions using Eclat at 5%, 7.5%, 10%, 12.5%, and 15%;
8. requires the independent 10% support counts to match the fresh negFIN outputs exactly; and
9. compares fresh results with the archived `neqFin_outputs/` and `summary_all_cities.xlsx` when those references are present.

The old UMAP/HAC workflow from `Euro_analysis.ipynb` was **not** imported. The v3 notebook continues to use Ward clustering directly in the original exact-support feature space and UMAP only for visualization and diagnostics.

## Functional validation of the added negFIN stage

The new negFIN-generation stage was tested against the repository inputs without re-running the full manuscript experiment.

Observed checks:

- transaction files processed: 100;
- fresh negFIN output files generated: 100;
- total city-itemset results at 10%: 17,847;
- union of 10% itemsets: 1,543;
- fresh negFIN vs archived `neqFin_outputs/`: 100/100 cities matched by parsed itemset, exact support count, and rounded percentage;
- fresh rounded negFIN summary vs `summary_all_cities.xlsx`: maximum absolute difference `0.0`;
- fresh exact negFIN support matrix vs independent Eclat 10% matrix: maximum absolute difference `0.0`;
- all notebook code cells pass static Python compilation.

The complete clustering/stability/UMAP experiment was not re-executed for this notebook-editing step; the existing embedded manuscript-run outputs were preserved.

## Scientific v3 corrections retained

The notebook retains the final v3 analytical design:

1. clustering is performed directly in the original 1,543-dimensional exact-support feature space after row-wise L2 normalization;
2. UMAP is used only for visualization and diagnostics;
3. the corrected nearest-10 UMAP overlap excludes each city itself;
4. cluster-number evaluation uses 500 paired 80%-feature subsampling repetitions;
5. `k=7` is retained by the declared equal-weight composite ranking, while `k=6` and `k=7` are described as nearly tied in feature stability;
6. exact support sensitivity is evaluated at 5%, 7.5%, 10%, 12.5%, and 15%;
7. representation tests include integer rounding, binary presence, counts, no row normalization, cosine/average linkage, PCA, removal of itemsets containing road code `12220`, artificial-only itemsets, and one vote per unique transaction pattern;
8. non-artificial transaction measures are interpreted descriptively rather than as land-area shares;
9. country-cluster association is treated as descriptive/hypothesis-generating rather than causal; and
10. unavailable geometry, same-class multiplicity, multi-buffer, and Core-versus-FUA tests are not claimed.

## Reference and fresh output separation

`JPER_outputs_v3/` contains the final results used in the manuscript. It is protected from overwrite by the notebook.

`JPER_reproduced_outputs_v3/` is a disposable fresh-run directory. It is deleted and recreated at the beginning of each Run All.

The final notebook writes `reference_reproduction_check.csv` and `output_manifest.csv` for auditability.
