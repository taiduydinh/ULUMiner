# ULUMiner — JPER v3 reproducibility package

This repository contains the inputs, code, notebook, city-map images, and final reference outputs supporting the revised manuscript **“Exploring Urban Land Use Patterns by Pattern Mining and Unsupervised Learning.”**

## Quick start

Keep the repository layout unchanged:

```text
ULUMiner/
├── cities/                         # 100 city-map images
├── inputs/                         # 100 focal-neighborhood transaction text files
├── JPER_outputs_v3/                # final reference outputs used in the paper
├── neqFin_outputs/                 # 100 saved negFIN output files used for validation
├── DP_Novak.pdf                    # documentary thesis source
├── JPER_v3.ipynb                   # main reproducibility notebook
├── JPER_v3_notes.md                # revision/reproducibility notes
├── negFIN.py                       # negFIN implementation/source
├── README.md
├── requirements_v3.txt
└── summary_all_cities.xlsx         # independent saved 10% city-by-itemset matrix
```

Then:

1. Install Python 3.10 (the retained final manuscript run used Python 3.10.15).
2. From the repository root, install the required packages:

   ```bash
   pip install -r requirements_v3.txt
   ```

3. Start Jupyter from the repository root:

   ```bash
   jupyter lab
   ```

4. Open `JPER_v3.ipynb`.
5. Choose **Run All**.
6. The notebook creates a new folder named `JPER_reproduced_outputs_v3/`.
7. At the end, inspect `JPER_reproduced_outputs_v3/reference_reproduction_check.csv` to compare key reproduced outputs with the final reference results included in `JPER_outputs_v3/`.

The notebook deliberately does **not** overwrite `JPER_outputs_v3/`.

## What each folder/file is for

- `inputs/`: the 100 focal-polygon neighborhood transaction files used as the primary analysis input.
- `neqFin_outputs/`: the 100 saved negFIN outputs used to validate exact re-mining.
- `JPER_outputs_v3/`: the final v3 outputs used in the revised paper and supplementary material. Treat this folder as read-only reference material.
- `cities/`: city-map images used by the optional image-thumbnail UMAP visualization at the end of the notebook.
- `summary_all_cities.xlsx`: independently archived 10% city-by-itemset matrix used for validation.
- `negFIN.py`: negFIN implementation/source.
- `DP_Novak.pdf`: documentary source used for historical workflow/buffer provenance.
- `JPER_v3_notes.md`: notes describing the v3 corrections and public-repository portability changes.
- `requirements_v3.txt`: package versions for the closest reproduction of the retained final run.

## Input integrity checks

Before analysis, `JPER_v3.ipynb` verifies:

- exactly 100 transaction files in `inputs/`;
- exactly 100 matching saved negFIN files in `neqFin_outputs/`;
- deterministic SHA-256 signatures for both complete directories;
- the SHA-256 checksum of `summary_all_cities.xlsx`; and
- when `JPER_outputs_v3/source_file_metadata.csv` is available, the per-file transaction hashes recorded in the final reference outputs.

If the inputs do not match the final v3 dataset, the notebook stops rather than silently continuing with mixed or altered data.

## Primary analysis reported in the paper

The notebook reproduces the analysis reported in the revised manuscript:

- 100 selected European urban areas;
- 290,396 focal-polygon neighborhood transactions;
- exact 10% minimum support for the primary representation;
- 1,543 common itemset-support features;
- row-wise L2 normalization;
- Ward hierarchical clustering directly in the original 1,543-dimensional feature space;
- candidate cluster numbers `k=2,...,10`;
- retained descriptive solution `k=7`;
- 500 paired 80%-feature subsampling repetitions;
- exact support-threshold sensitivity at 5%, 7.5%, 10%, 12.5%, and 15%;
- representation and clustering sensitivity, including road-code removal and artificial-only features; and
- 54 UMAP parameter/seed combinations used only for visualization and diagnostics.

UMAP coordinates are **not** used to define the primary cluster assignments.

## Reference outputs and fresh reproduction outputs

`JPER_outputs_v3/` contains the final outputs used for the paper. These are included so users can inspect the published/reference results without rerunning the notebook.

A fresh **Run All** writes to:

```text
JPER_reproduced_outputs_v3/
```

This separation prevents an accidental rerun from changing the reference results.

The final notebook comparison cell writes:

```text
JPER_reproduced_outputs_v3/reference_reproduction_check.csv
```

which compares key reproduced results with `JPER_outputs_v3/`.

## About the displayed notebook results

The outputs already displayed inside `JPER_v3.ipynb` are retained from the authors' final manuscript run. The repository was renamed/reorganized afterward for easier public use, so a few retained path messages may show earlier local paths or earlier folder names. This does **not** affect the stored numerical results. Running the notebook refreshes those messages using the current folder layout above.

## Interpretation limits

Each transaction is an unordered set of land-use codes around one focal polygon. Repeated transaction rows are retained and contribute repeatedly to support, preserving prevalence across focal neighborhoods. Duplicate occurrences of the same class within one transaction were already collapsed upstream.

The archived transaction files do not retain polygon area, geometry, same-class neighbor multiplicity, shared-boundary length, orientation, exact distance, or overlap extent. These unavailable quantities are not reconstructed or imputed.

The seven clusters should therefore be interpreted as descriptive comparative groupings conditional on the stated representation and analytical choices, not as causal planning-system categories, normative rankings, or universal urban typologies.
