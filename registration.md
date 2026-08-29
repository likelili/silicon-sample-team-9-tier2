# Silicon Sample Benchmark — method registration form

This registration documents Team 9's Tier 2 `secondary-1` submission. Items marked **★** are public; items marked **†** are available through escrow.

## 0 · Approach identity and output

- **0.1 Team ★:** Team 9 (ExploraTwin): Olivier Toubia, Tianyi Peng, George Gui, Yuchen Qiu, and Naveen Venkat. Affiliations and emails appear in `metadata.json`; Olivier Toubia (`ot2107@gsb.columbia.edu`) is the corresponding contact. The organizers approved this five-person team.
- **0.2 Plain-language summary ★:** We simulated every condition with GPT-5.6 Luna digital twins based on Twin-2K-500, calibrated each of the 221 condition-outcome columns with the SYN-DIGITS elastic-net estimator and 123 Twin-2K Wave-4 anchors, and poststratified the calibrated individual predictions to estimate condition and one-way moderator means.
- **0.3 Tier and approach family ★:** Tier 2; calibrated and poststratified individual digital-twin simulations aggregated to group-level predictions.
- **0.4 Ordered pipeline:** (1) We parsed the supplied QSF and simulated 2,007 eligible twins in all 17 conditions. (2) We retained the 1,921 twins with exactly complete responses in every condition. (3) We constructed the 13 benchmark outcomes before calibration. (4) We aligned each twin's 123 Wave-4 synthetic anchor responses with the corresponding human Twin-2K responses. (5) For each of the 17 × 13 targets, we fit the frozen elastic-net specification on the synthetic anchor columns and synthetic Silicon target, then applied the fitted relationship to the human anchor matrix. (6) We assigned one fixed poststratification weight to each clean-pool twin using the 40-cell gender × age × race target table. (7) We used the full weighted pool to estimate 221 condition means and 5,967 one-way moderator means.
- **0.5 Coverage ★:** All 17 conditions, 13 outcomes, six moderators, and 27 moderator levels. The main and moderator files contain 221 and 5,967 unique, nonmissing rows, respectively.

## A · Scope of LLM use

- **A.1 Purpose:** GPT-5.6 Luna generated pre-study, study, and Wave-4 anchor responses. Calibration and aggregation were deterministic.
- **A.2 Automation ★:** Prediction generation, calibration, and aggregation were automated. No simulated or calibrated value was manually edited.

## B · Model and system details

- **B.1 Model:** OpenAI `gpt-5.6-luna`, reasoning effort `medium`.
- **B.2 Access:** Pre-study and Wave-4 anchors used the synchronous Responses API; study runs used `chat/completions` through the Batch API on August 26–28, 2026.
- **B.3 Configuration:** Temperature and top-p were not sent. Survey randomization used global seed 42 and deterministic twin-condition seeds. Provider-side generation was not seedable.
- **B.4 Customization:** No fine-tuning, retrieval, tools, or web access.
- **B.5 Memory:** No provider memory. Frozen pre-study answers were inserted explicitly into each study prompt.
- **B.6 Inference stack:** Hosted API.
- **B.7 Ensembles:** None.

## C · Prompts

- **C.1 Exact prompts:** Survey prompts were produced by the deposited SurveyTwin orchestration code; the request and response archive is escrowed.
- **C.2 System instructions:** The model answered as the supplied persona and returned structured JSON matching displayed options or numeric ranges.
- **C.3 Design:** Prompts preserved QSF flow, randomization, stimuli, and prior-stage answers.

## D · Persona and profile construction

- **D.1 Source:** Twin-2K-500 contains more than 500 survey answers from a representative panel of 2,058 U.S. respondents (Toubia et al. 2025). The pre-study used full personas; study and Wave-4 simulations used summary personas.
- **D.2 Verbalization:** We used the released full and narrative-summary representations without editing them.
- **D.3 Assignment and weighting:** Each eligible twin completed every condition, and calibration was fit on the universal clean pool of 1,921 twins. We poststratified that full pool to a 40-cell gender × age × race table. The table was reconstructed by iterative proportional fitting of a Census-seeded joint distribution to the benchmark's released gender × age and gender × race margins. Those released margins are reproduced exactly; the unreported age × race association is modeled rather than benchmark-supplied. Each twin's fixed weight equals its target-cell proportion divided by its clean-pool cell proportion and is reused in every condition and outcome.

## E · Stimulus and survey administration

- **E.1 Presentation:** Each session displayed one QSF-defined intervention or control text. The state-dependent “Extreme weather predictions” condition used two stages.
- **E.2 Walk-through:** Sixteen conditions used one study call; “Extreme weather predictions” used two. QSF block, outcome, and choice randomization used deterministic session seeds.
- **E.3 Elicitation:** The model returned `question_id`, `answer_value`, and `answer_label` in JSON.

## F · Stochasticity and aggregation

- **F.1 Runs and seeds:** One run per twin-condition combination; global seed 42. All calibration calculations were deterministic.
- **F.2 Aggregation:** Each condition mean is the weighted mean of calibrated predictions over all 1,921 clean-pool twins. For a one-way moderator cell, we restrict the pool to that moderator level and renormalize the same fixed weights within the subset. This preserves the target joint distribution conditionally for age, gender, and race and applies the same population adjustment within education, income, and party subgroups. Twin-2K has no respondents in the benchmark's `gender = Other` level; following organizer guidance, those 221 cells repeat the corresponding condition mean as an explicit no-moderation prediction.

## G · Validation and post-processing

- **G.1 Human validation:** No manual response editing or human outcome ratings.
- **G.2 Parsing and exclusions:** We accepted only sessions whose returned question-ID set exactly equaled the requested set. The production reconstruction matched the Tier 1 export in all 117,000 checked outcome cells. The weighting audit recovers all 40 target-cell proportions to numerical precision; weights range from 0.309 to 9.792 and yield an effective sample size of 1,442.7. Every submitted cell is unique, nonmissing, and within its declared native scale.
- **G.3 Calibration:** We used the SYN-DIGITS elastic-net estimator with `regularization_multiplier = 0.01`, `l1_ratio = 0.3`, rank-5 hard-SVD donor imputation, `min_col_std = 1.0`, and all 123 Wave-4 anchors. For each target, the model was fit on synthetic anchors plus that synthetic Silicon outcome and transferred to the aligned human anchor matrix. Rows missing a synthetic target were excluded from that target's fit rather than mean-filled. Calibration was applied to every target; no adaptive threshold was used. A fixed `tau = 0.15` gate worsened tuning panel-mean error, and a separate MegaStudy check found no stable threshold that improved on both raw and always-calibrate predictions. The frozen no-gate procedure was then evaluated once on 61 untouched Wave-4 anchors: normalized panel-mean absolute error decreased by 0.259 percentage points and subgroup RMSE by 0.600 points relative to raw DT predictions.

## H · Learning and conditioning components

- **H.1 Fine-tuning:** None.
- **H.2 Calibration data:** The donor matrices contain 123 Twin-2K Wave-4 items aligned at the respondent and item levels: human responses from the original panel and GPT-5.6 Luna responses generated from summary personas. Each of the 221 Silicon outcomes was a separate target; Silicon outcomes were never donors for other targets.

## I · Data inputs, blinding, and competing interests

- **I.1 Competing interests ★:** API costs were paid by Columbia Business School. The team has no relationship with OpenAI beyond a paid API account.
- **I.2 External human data †:** Twin-2K human answers were used as calibration anchors. They contain no Silicon Sample benchmark outcomes.
- **I.3 Blinding ★:** No team member accessed human benchmark outcomes before the prediction lock. Olivier Toubia signed the no-exposure declaration on August 18, 2026.
- **I.4 Contamination †:** OpenAI had not publicly disclosed a training-data cutoff for `gpt-5.6-luna`. Benchmark human outcomes were unpublished before the lock.

## J · Design-space search

- **J.1 Search †:** Before production, eight estimator families were compared on a prespecified half of the 123 Wave-4 anchors. Production nevertheless used the published Twin-2K elastic-net specification to remain tied to the SYN-DIGITS implementation. Gate behavior was examined only on the Wave-4 tuning fold and separate MegaStudy outcomes; the 61-anchor holdout was opened once after freezing the no-gate rule. No Silicon human outcome was used.

## K · Reproducibility and frozen artifacts

- **K.1 Code and materials:** Public code and derived matrices are in this repository. `code/calib/reproduce_submission.py` regenerates the 221 elastic-net calibrations from the public matrices and verifies both submitted files; `code/calib/poststratification.py` constructs and audits the fixed 40-cell weights. The original response-to-target reconstruction remains in `code/calib/production.py` and uses the escrowed run archive. The estimator implementation is vendored under its MIT license in `code/syn-digits/`.
- **K.2 Raw logs †:** The Silicon simulation request and response archive and SurveyTwin engine snapshot are under restricted access at [10.5281/zenodo.22150315](https://doi.org/10.5281/zenodo.22150315). Public `artifacts/` contains the aligned anchor matrices, raw and calibrated 1,921 × 221 target matrices, target grid, respondent weights, and poststratification audit needed to reproduce the submitted means.
- **K.3 Resources:** The calibration fit 221 elastic-net target models on 1,921 twins using one workstation. Calibration itself used no API calls.

## L · Disclosure class

**Class B (escrowed).** Prediction files, calibration code, anchors, target matrices, and audits are public; full simulation request/response logs and the engine snapshot are escrowed at [10.5281/zenodo.22150315](https://doi.org/10.5281/zenodo.22150315).

## References

Peng, Tianyi et al. (2026), “Calibrated Digital Twins,” arXiv:2604.07513. [https://doi.org/10.48550/arXiv.2604.07513](https://doi.org/10.48550/arXiv.2604.07513).

Toubia, Olivier, George Z. Gui, Tianyi Peng, Daniel J. Merlau, Ang Li, and Haozhe Chen (2025), “Database Report: Twin-2K-500,” *Marketing Science*, 44 (6), 1446–1455. [https://doi.org/10.1287/mksc.2025.0262](https://doi.org/10.1287/mksc.2025.0262).
