# Team 9 — Silicon Sample Benchmark Tier 2 Submission

This repository contains Team 9's `secondary-1` Tier 2 submission. We calibrated GPT-5.6 Luna digital-twin outcomes with 123 Twin-2K Wave-4 anchors using the SYN-DIGITS elastic-net estimator. We then estimated condition and moderator-level means from the full 1,921-person clean pool using fixed poststratification weights for a 40-cell gender × age × race target distribution.

## Submission files

- `predictions/team_9_T2_secondary-1_v1_cells_main.csv` — 221 condition × outcome means; SHA-256 `3a89a948cd4538043c8da9886a6eec441bbf469583efc9cf70d8bc89d66829e4`.
- `predictions/team_9_T2_secondary-1_v1_cells_moderator.csv` — 5,967 condition × moderator-level × outcome means; SHA-256 `ee1189138e7c43bcb5c8406177385612183d552c7a403195420e7ac8516d2df8`.
- `registration.md` and `metadata.json` — method and submission metadata.
- `code/calib/` and `code/syn-digits/` — calibration and estimator code.
- `artifacts/` — aligned Wave-4 anchors, raw and calibrated Silicon target matrices, the 40-cell target grid, respondent weights, fit diagnostics, and audit reports.

Run the organizers' validator with `make check`. To regenerate the elastic-net calibration and verify the submitted means from the public matrices, run `PYTHONPATH=code python -m calib.reproduce_submission` in an environment with the listed scientific dependencies. The simulation request and response archive is available under restricted access at [Zenodo](https://doi.org/10.5281/zenodo.22150315).

Authors: Olivier Toubia, Tianyi Peng, George Gui, Yuchen Qiu, and Naveen Venkat. Corresponding contact: Olivier Toubia (`ot2107@gsb.columbia.edu`).
