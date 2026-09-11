# ULUMiner — JPER v3 reproducibility package

This repository contains the inputs, code, final reference outputs, and notebook supporting the revised manuscript **“Exploring Urban Land Use Patterns by Pattern Mining and Unsupervised Learning.”**

The public notebook is designed so that a user can start from the transaction files, run the included negFIN implementation, reconstruct the primary frequent-itemset representation, and reproduce the subsequent clustering and sensitivity analyses.

## Repository layout

Keep these names unchanged:

```text
ULUMiner/
├── cities/                         # 100 city-map images
├── inputs/                         # 100 focal-neighborhood transaction text files
├── JPER_outputs_v3/                # final reference outputs used in the paper
├── neqFin_outputs/                 # archived 10% negFIN outputs for validation
├── ProjectArcGIS/                  # ArcGIS Pro sample project for transaction generation
├── DP_Novak.pdf                    # documentary thesis source
├── JPER_v3.ipynb                   # main end-to-end reproducibility notebook
├── JPER_v3_notes.md                # revision/reproducibility notes
├── negFIN.py                       # negFIN implementation
├── README.md
├── requirements_v3.txt
└── summary_all_cities.xlsx         # archived rounded 10% city-by-itemset matrix
```

`JPER_outputs_v3/` and `neqFin_outputs/` are reference material. A fresh run never overwrites them.

## Quick start

Python 3.10 is recommended for the closest reproduction of the retained final manuscript run.

Create and activate an environment, then install the required packages:

```bash
pip install -r requirements_v3.txt
```

Start Jupyter from the repository root:

```bash
jupyter lab
```

Open:

```text
JPER_v3.ipynb
```

and choose **Restart Kernel and Run All Cells**.

The notebook creates:

```text
JPER_reproduced_outputs_v3/
```

All newly generated files are written there.

## What happens during Run All

The notebook performs the following steps in order:

1. verifies the 100 files in `inputs/` and the included `negFIN.py`;
2. parses and audits all transaction rows;
3. runs `negFIN.py` at 10% relative minimum support on all 100 cities;
4. writes the fresh negFIN results to `JPER_reproduced_outputs_v3/negFIN_outputs_10pct/`;
5. rebuilds `summary_all_cities_from_negFIN_10pct.xlsx`;
6. independently re-mines exact frequent itemsets at 5%, 7.5%, 10%, 12.5%, and 15% using an Eclat verifier;
7. checks the fresh 10% negFIN support counts against the independent re-mining;
8. when the archived validation files are present, compares the fresh negFIN results with `neqFin_outputs/` and the rounded summary with `summary_all_cities.xlsx`;
9. constructs the exact 100 × 1,543 primary city-by-itemset support matrix;
10. performs Ward hierarchical clustering directly in the original L2-normalized feature space;
11. evaluates `k=2,...,10`, including 500 paired feature-subsampling repetitions;
12. performs representation, support-threshold, road, and non-artificial-land-use sensitivity analyses;
13. runs the 54 UMAP parameter/seed combinations for diagnostic/visualization purposes; and
14. compares key reproduced outputs with the reference results in `JPER_outputs_v3/`.

UMAP coordinates are **not** used to define the primary clusters.

## negFIN outputs

The included `negFIN.py` is run directly by the notebook.

Primary setting:

```text
relative minimum support = 0.10
```

A fresh run creates:

```text
JPER_reproduced_outputs_v3/negFIN_outputs_10pct/
```

The repository's existing:

```text
neqFin_outputs/
```

is the archived 10% negFIN result set used for validation only.

The notebook loads a fresh copy of `negFIN.py` for each city because the implementation uses module-level state. This reproduces the behavior of the earlier analysis notebook, which reloaded the module between datasets.

## Exact support versus rounded support

Each negFIN output line contains:

```text
<itemset> #SUP:<exact integer count> %:<rounded whole percent>
```

The revised paper uses **exact relative support**:

```text
exact support count / number of transactions in the city
```

It does not use the rounded `%:` value for the primary clustering.

The rounded field is reconstructed only to reproduce the historical `summary_all_cities.xlsx` representation and to evaluate rounding sensitivity.

## Main output files from a fresh run

Among the files produced in `JPER_reproduced_outputs_v3/` are:

```text
negFIN_outputs_10pct/
negFIN_generation_audit.csv
negFIN_run_log.txt
negFIN_generated_vs_reference_validation.csv
summary_all_cities_from_negFIN_10pct.xlsx
negFIN_exact_support_matrix_10pct.csv.gz
raw_transaction_audit.csv
city_itemset_exact_support_10pct.csv.gz
primary_cluster_assignments.csv
primary_k_quality.csv
primary_feature_stability_paired_runs.csv
primary_feature_subsample_plan.csv
exact_support_threshold_sensitivity.csv
representation_and_geometry_sensitivity.csv
umap_all_54_runs.csv
umap_overall_diagnostic_ranges.csv
cluster_characteristic_itemsets.csv
reference_reproduction_check.csv
output_manifest.csv
```

The complete output inventory is written to `output_manifest.csv`.

## Reference outputs versus reproduced outputs

`JPER_outputs_v3/` contains the final v3 outputs used in the paper and supplementary material. Treat it as read-only.

A fresh user run writes to:

```text
JPER_reproduced_outputs_v3/
```

At the end, `reference_reproduction_check.csv` compares key reproduced tables with the included reference results.

Deterministic/core result mismatches stop the notebook. UMAP comparison differences are reported as warnings because even seeded UMAP can show small platform/library differences.

## Input integrity

The notebook verifies the final transaction dataset using a deterministic SHA-256 directory signature. It also verifies the exact `negFIN.py` implementation.

When present, it additionally validates:

- all 100 archived files in `neqFin_outputs/`;
- `summary_all_cities.xlsx`; and
- per-file source hashes stored in `JPER_outputs_v3/source_file_metadata.csv`.

This prevents silently mixing files from different experiment versions.

## City-map images

`cities/` contains the city images used by the final image-based UMAP visualization.

The numerical analysis does not depend on these images. If `cities/` is absent, the notebook completes the analytical workflow and skips only the optional thumbnail visualization.

## ArcGIS Pro preprocessing example

The `ProjectArcGIS/` folder provides a worked ArcGIS Pro example showing how the spatial Urban Atlas polygons can be converted into transaction data before frequent-itemset mining.

The example project was prepared and functionally checked using **Olomouc** data. It includes the processing model derived from the workflow developed by Pavel Novak and a short `info.txt` file describing the contents of the folder.

This ArcGIS Pro example is provided for transparency and for users who want to inspect or reproduce the **spatial preprocessing stage** that precedes the Python notebook. In particular, the project demonstrates the construction of focal-polygon neighborhood transactions using the study's **100 m distance setting**. The distance parameter can also be changed in ArcGIS Pro, which makes the project useful for future experiments with alternative neighborhood distances.

The ArcGIS project is **not required** to reproduce the numerical results reported in the current paper. `JPER_v3.ipynb` starts from the already generated transaction files in `inputs/`. Therefore, users interested only in reproducing the published mining, clustering, and sensitivity analyses can run the notebook directly. Users interested in the upstream GIS workflow can inspect `ProjectArcGIS/` separately.

Because the final archived 100-city transaction files do not contain polygon geometry or alternative-buffer versions, the current paper does not claim a new multi-city buffer-distance robustness experiment. The `ProjectArcGIS/` example provides a practical basis for such future extensions.

## Reproducibility notes

The outputs already displayed inside `JPER_v3.ipynb` are retained from the authors' final manuscript run. The notebook source was subsequently reorganized for a cleaner public workflow and the explicit negFIN-generation cells were added. Therefore newly added cells do not contain retained output until a user executes **Run All**.

The reference results in `JPER_outputs_v3/` were not regenerated or changed by this repository-portability update.

## Interpretation limits

Each transaction is an unordered set of land-use codes around one focal polygon. Repeated transaction rows are retained and contribute repeatedly to support, preserving prevalence across focal neighborhoods. Duplicate occurrences of the same class within one transaction were already collapsed upstream.

The transaction files do not retain polygon area, original geometry, same-class neighbor multiplicity, shared-boundary length, orientation, exact distance, or overlap extent. These unavailable quantities are not reconstructed or imputed.

The seven clusters should therefore be interpreted as descriptive comparative groupings conditional on the stated representation and analytical choices, not as causal planning-system categories, normative rankings, or universal urban typologies.

## Citation

If you use the data, code, notebook, or results from this repository, please cite the associated JPER manuscript:

> Dobesova, Zdena, Tai Dinh, and Pavel Novak. “Exploring Urban Land Use Patterns by Pattern Mining and Unsupervised Learning.” *Journal of Planning Education and Research (JPER)*, 2026. Manuscript submitted for publication.

BibTeX:

```bibtex
@article{dobesova2026exploring,
  author  = {Dobesova, Zdena and Dinh, Tai and Novak, Pavel},
  title   = {Exploring Urban Land Use Patterns by Pattern Mining and Unsupervised Learning},
  journal = {Journal of Planning Education and Research},
  year    = {2026},
  note    = {Manuscript submitted for publication}
}
```

Please update the bibliographic details (volume, issue, pages, and DOI) after publication.

