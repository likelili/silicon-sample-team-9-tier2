# Team 9 — Silicon Sample Benchmark Tier 2 Submission

This repository contains Team 9's `secondary-1` Tier 2 submission. We calibrated GPT-5.6 Luna digital-twin outcomes with 123 Twin-2K Wave-4 anchors using the SYN-DIGITS elastic-net estimator, then aggregated the calibrated individual predictions into condition and moderator-level means.

## Submission files

- `predictions/team_9_T2_secondary-1_v1_cells_main.csv` — 221 condition × outcome means; SHA-256 `d19f7e021945db59e0b5c20868d4abdf2fae06a7e1fef723ace1b61c12b1eef3`.
- `predictions/team_9_T2_secondary-1_v1_cells_moderator.csv` — 5,967 condition × moderator-level × outcome means; SHA-256 `1d70e5655dc5cf5d3253d4e269175957b8b15cf2ae8756cf0556f7e4ab14748f`.
- `registration.md` and `metadata.json` — method and submission metadata.
- `code/calib/` and `code/syn-digits/` — calibration and estimator code.
- `artifacts/` — aligned Wave-4 anchors, raw and calibrated Silicon target matrices, fit diagnostics, and audit reports.

Run the organizers' validator with `make check`. The simulation request and response archive is available under restricted access at [Zenodo](https://doi.org/10.5281/zenodo.22150315).

Authors: Olivier Toubia, Tianyi Peng, George Gui, Yuchen Qiu, and Naveen Venkat. Corresponding contact: Olivier Toubia (`ot2107@gsb.columbia.edu`).
