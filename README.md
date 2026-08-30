# Team 9 — Silicon Sample Benchmark Tier 2 Submission

This repository contains Team 9's `secondary-1` Tier 2 submission. We calibrated GPT-5.6 Luna digital-twin outcomes with 123 Twin-2K Wave-4 anchors using the SYN-DIGITS framework of Fan et al. (2026) and its elastic-net specification with the published adaptive-transfer gate (`tau = 0.15`). The gate keeps the calibrated prediction for 68 of the 221 targets and the uncalibrated digital-twin response for the remaining 153. We then estimated condition and moderator-level means from the full 1,921-person clean pool by reweighting individuals to a 40-cell gender × age × race target distribution.

## Submission files

- `predictions/team_9_T2_secondary-1_v1_cells_main.csv` — 221 condition × outcome means; SHA-256 `9f50e885aee5067460eee2d5ef2de0259f35dda237ffe50a8b4d36cbc71b076d`.
- `predictions/team_9_T2_secondary-1_v1_cells_moderator.csv` — 5,967 condition × moderator-level × outcome means; SHA-256 `11743092d5e75504c53b057bad6e4aa5fd4befa0329ec0993175b87e29c623c9`.
- `registration.md` and `metadata.json` — method and submission metadata.
- `code/calib/` and `code/syn-digits/` — calibration and weighting code.
- `artifacts/` — aligned Wave-4 anchors, raw and calibrated Silicon target matrices, the 40-cell target grid, respondent weights, fit diagnostics, and audit reports.

Run the organizers' validator with `make check`. To regenerate the elastic-net calibration and verify the submitted means from the public matrices, run `PYTHONPATH=code python -m calib.reproduce_submission` in an environment with the listed scientific dependencies. The adopted provider responses and supporting reproduction materials are available under restricted access at [Zenodo](https://doi.org/10.5281/zenodo.22168892).

Authors: Olivier Toubia, Tianyi Peng, George Gui, Yuchen Qiu, and Naveen Venkat.
