# Public repository portability update — 2026-09-10

The distributed notebook source was updated for the final GitHub layout while preserving all embedded outputs from the authors' final manuscript run. No scientific analysis was re-executed for this portability edit.

- Reads transactions directly from `inputs/`.
- Reads saved negFIN results directly from `neqFin_outputs/` (with conventional spelling fallbacks).
- Reads `summary_all_cities.xlsx` from the repository root.
- Verifies deterministic directory signatures for the 100 transaction files and 100 saved negFIN files.
- Protects `JPER_outputs_v3/` as read-only reference outputs; fresh runs write to `JPER_reproduced_outputs_v3/`.
- Compares key reproduced CSV results with the included manuscript reference outputs at the end of a run.
- Uses `cities/` for the city-thumbnail UMAP. If that folder is omitted in another copy of the repository, the numerical analysis still completes and the optional thumbnail figure is skipped cleanly.
- `requirements_v3.txt` now reflects the package versions recorded in the retained final v3 manuscript run.

---

# JPER v3 notebook patch notes

Prepared: 2026-09-05

## File

`JPER_v3.ipynb`

Derived from authoritative v2 notebook SHA-256:
`d8b1f635d08b182f3048b84f451b6a9d525404b64123aac5be2d4ba7722be43b`

## Scientific/reproducibility corrections

1. Corrects the UMAP nearest-neighbor overlap helper so it evaluates the nearest 10 *other* cities, not neighbors ranked 2–11.
2. Replaces the v2 50-run, unpaired-by-k feature perturbation design with 500 paired 80%-feature perturbations. Each sampled feature subset is reused across every k=2...10.
3. Preserves k=7 as the primary solution under the declared equal-weight composite ranking, while explicitly reporting that k=6 and k=7 are nearly tied in feature stability.
4. Seeds rural plot jitter deterministically.
5. Labels rural-content Kruskal-Wallis p-values as exploratory/descriptive because the metrics and clusters derive from the same transactions.
6. Adds input checksum guards for the authoritative source archive and summary workbook.
7. Adds warnings when installed package versions differ from the validated pins.
8. Adds `umap_overall_diagnostic_ranges.csv` for corrected 54-run ranges.
9. Adds `primary_feature_stability_paired_runs.csv` and `primary_feature_subsample_plan.csv` for auditability.
10. Adds generated-PDF integrity checks and verifies the non-self output manifest after writing it.
11. Adds modern notebook cell IDs and clears stale v2 execution outputs.
12. Adds manuscript reporting warnings covering sampling provenance, negFIN complexity notation, buffer/transaction-count wording, and the exploratory status of same-data cluster tests.

## Independently checked during construction

- Notebook format: nbformat 4.5.
- 34 total cells, 20 code cells.
- All cell IDs present and unique.
- All 20 code cells compile successfully.
- Authoritative source archive checksum passes.
- Authoritative summary workbook checksum passes.
- 100 cities and 290,396 transaction rows reproduce.
- Exact 10% matrix contains 1,543 itemsets.
- Zero discrepancies against all saved negFIN outputs.
- Primary seven-cluster sizes remain 35, 8, 6, 18, 6, 18, 9.
- 500 paired stability means independently reproduced: k=6 = 0.7615964014; k=7 = 0.7601263447.
- With the revised stability term, k=7 remains the best eligible equal-weight composite rank (3.50), but is not uniquely the stability winner.

A full 54-run UMAP execution was not performed in the construction container because the pinned `umap-learn==0.5.9.post2` environment is not installed there. Run the notebook under the package's pinned environment for final corrected UMAP ranges.
