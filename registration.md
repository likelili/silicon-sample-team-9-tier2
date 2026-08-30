# Silicon Sample Benchmark — method registration form

This registration documents Team 9's Tier 2 `secondary-1` submission. Items marked **★** are public; items marked **†** are available through escrow.

## 0 · Approach identity and output

- **0.1 Team ★:** Team 9 (ExploraTwin): Olivier Toubia, Tianyi Peng, George Gui, Yuchen Qiu, and Naveen Venkat. The organizers approved this five-person team.
- **0.2 Plain-language summary ★:** We used Twin-2K-500 digital twins to simulate responses in every benchmark condition. We calibrated the individual predictions using matched human and synthetic responses to previously administered questions through SYN-DIGITS (Fan et al. 2026), then applied demographic cell-proportion weights to estimate condition-level and subgroup means.
- **0.3 Submission tier and approach family ★:** Tier 2; single-model, persona-conditioned survey simulation followed by deterministic calibration and demographic reweighting.
- **0.4 Pipeline diagram:** (1) We screened the Twin-2K respondents using their pre-study responses. (2) We simulated every eligible twin in all 17 conditions. (3) We retained the twins with complete responses in every condition and constructed the 13 benchmark outcomes from their survey answers. (4) We calibrated each condition–outcome prediction using matched human and synthetic Wave-4 responses. (5) We applied fixed demographic weights and aggregated the individual predictions into overall condition means and one-way moderator means.
- **0.5 Coverage ★:** Complete coverage of all 17 conditions and 13 outcomes. The submission contains 221 condition–outcome means and 5,967 one-way moderator means covering all six moderators and 27 reported levels; every required prediction is unique and nonmissing.

## A · Scope of LLM use

- **A.1 Purpose:** GPT-5.6 Luna generated the twins' pre-study responses, benchmark-study responses, and separate Wave-4 anchor responses. All eligibility checks, outcome construction, calibration, weighting, and aggregation were performed deterministically without an LLM.
- **A.2 Degree of automation ★:** The workflow was fully automated at prediction time. No simulated response, calibrated prediction, or submitted mean was manually selected or edited.

## B · Model / system details

- **B.1 Model name:** OpenAI `gpt-5.6-luna`, using the exact provider model identifier configured by the platform.
- **B.2 Access and context mode:** Hosted API. Pre-study and Wave-4 calls used the synchronous Responses API; benchmark-study calls used `chat/completions` through the Batch API on August 26–28, 2026. Each condition was a fresh session.
- **B.3 Configuration:** Medium reasoning effort and one completion per request. The study batch requests set `max_completion_tokens = 8000`. Temperature, top-p, top-k, frequency and presence penalties, stop sequences, and provider generation seeds were not supplied. Survey-side randomization used global seed 42 and deterministic session seeds.
- **B.4 Customization:** No fine-tuning, retrieval-augmented generation, prompt optimization against benchmark outcomes, tool use, web search, or agentic model orchestration.
- **B.5 Persistent memory:** None. The model retained no state across conditions. When earlier answers were required by the survey flow, they were inserted explicitly into the next prompt.
- **B.6 Inference stack:** Not applicable; the model was accessed through a hosted API rather than served locally.
- **B.7 Ensembles:** None; all simulated responses came from one model and one completion per request.

## C · Prompts

- **C.1 Exact prompts:** The exact system and user messages were generated from the supplied QSF by the frozen SurveyTwin pipeline. Full request bodies are included in the escrowed archive. Prompts were not revised in response to benchmark predictions.
- **C.2 System-wide instructions:** The model was instructed to answer as the supplied persona, follow the displayed survey instructions and response scales, and return one structured response for every requested answer unit.
- **C.3 Prompt-design rationale:** The prompts preserve the wording, stimuli, response options, order, and logic of the supplied instrument while requiring machine-readable output that can be aligned with Qualtrics variables and validated automatically.

## D · Persona / profile construction (Tiers 1–2)

- **D.1 Profile source:** We used Twin-2K-500, which contains more than 500 survey answers from a representative panel of 2,058 U.S. respondents (Toubia et al. 2025). Of these, 2,007 passed the prespecified pre-study eligibility checks. Every eligible twin was assigned to all 17 conditions rather than to only one condition.
- **D.2 Profile verbalization:** Pre-study eligibility responses were generated from the released full representation. Benchmark and Wave-4 simulations used the released paragraph-length summary representation. Neither representation was edited.
- **D.3 Assignment and weighting:** We retained the 1,921 twins with exactly complete sessions in every condition, so the same respondents underlie every contrast. No respondents were resampled for Tier 2. We assigned one fixed weight to each twin using a 40-cell gender × age × race target distribution. The joint distribution was reconstructed by iterative proportional fitting of a Census-seeded table to the benchmark's released gender × age and gender × race margins. Each weight equals the cell's target proportion divided by its proportion in the clean pool and is reused across conditions and outcomes.

## E · Stimulus and survey administration

- **E.1 Stimulus presentation:** Intervention and control texts were taken verbatim from the QSF. Each session received exactly one intervention or one of the three texts pooled as the control condition. For the state-contingent “Extreme weather predictions” condition, the applicable follow-up text was selected after the model answered the state question.
- **E.2 Survey walk-through:** Sixteen conditions were administered in one model call. The state-contingent condition used two sequential calls, with the first-stage answer and prior survey context supplied to the second stage. QSF block order, outcome order, choice order, and condition-specific randomization were resolved with deterministic session seeds. Questions, response scales, and attention or comprehension items were displayed as specified by the QSF.
- **E.3 Response elicitation:** The model returned constrained, structured JSON containing the question identifier, answer value, and answer label for each requested answer unit. No token log-probabilities were used.

## F · Stochasticity and aggregation

- **F.1 Runs and seeds:** We generated one response per twin–condition combination. Survey randomization used global seed 42 and deterministic session-level seeds; the provider did not expose deterministic generation for this model. Calibration and aggregation are exactly reproducible from the deposited matrices.
- **F.2 Aggregation rule:** For each condition–outcome pair, we took the weighted mean across all 1,921 clean-pool twins. For each one-way moderator prediction, we restricted the pool to that level and renormalized the same weights within the subset. Twin-2K contains no respondents in the benchmark's `gender = Other` level; following organizer guidance, these cells repeat the corresponding overall condition mean as a no-moderation prediction.

## G · Validation and post-processing

- **G.1 Human validation:** None. Humans did not review, rate, select, or edit the simulated responses or submitted predictions.
- **G.2 Post-processing:** Returned responses were parsed by question identifier and converted according to each item's declared choice or numeric scale. We retained only sessions whose returned question set exactly matched the requested set and only twins with exact completion in all 17 conditions. The resulting effective sample is 1,921 per condition. Reconstruction reproduced all 117,000 Tier 1 outcome cells, and every Tier 2 prediction is unique, finite, and within its declared scale. The demographic weights range from 0.309 to 9.792, have an effective sample size of 1,442.7, and recover the 40 target-cell proportions to numerical precision.
- **G.3 Calibration corrections:** We applied SYN-DIGITS (Fan et al. 2026) separately to each of the 221 condition–outcome columns. The calibration inputs were respondent-aligned human and GPT-5.6 Luna responses to 123 Twin-2K Wave-4 questions; the Wave-4 synthetic answers were generated separately and were not included in the benchmark personas. For each target, rank-5 hard-SVD imputation filled structurally missing anchor values separately in the human and synthetic matrices; each matrix was standardized using its own column statistics with `min_col_std = 1.0`. An elastic net with regularization multiplier `0.01` and L1 ratio `0.3` learned the relationship from synthetic anchors to the synthetic benchmark target, and that relationship was then applied to the human anchors. No human benchmark outcome was used. Following the published transfer rule, we retained the calibrated column when the synthetic-side training MSE was at most `0.15` and otherwise retained the raw digital-twin column; 68 targets were calibrated and 153 used the raw fallback. This specification was fixed without reference to human Silicon Sample outcomes.

## H · Learning and conditioning components

- **H.1 Fine-tuning:** None.
- **H.2 Calibration data:** The donor matrices contain 123 Twin-2K Wave-4 items aligned at the respondent and item levels: human responses from the original panel and GPT-5.6 Luna responses generated from summary personas. Each of the 221 Silicon outcomes was a separate target; Silicon outcomes were never donors for other targets.

## I · Data inputs, blinding, and competing interests

- **I.1 Competing interests ★:** API costs were paid by Columbia Business School. The team has no relationship with OpenAI beyond a paid API account.
- **I.2 External human data †:** Twin-2K human answers were used as calibration anchors. They contain no Silicon Sample benchmark outcomes.
- **I.3 Blinding ★:** No team member accessed human benchmark outcomes before the prediction lock. Olivier Toubia signed the no-exposure declaration on August 18, 2026.
- **I.4 Contamination †:** OpenAI had not publicly disclosed a training-data cutoff for `gpt-5.6-luna`. Benchmark human outcomes were unpublished before the lock.

## J · Design-space search

- **J.1 Search †:** Before production, eight estimator families were compared on a prespecified half of the 123 Wave-4 anchors. Production used the published Twin-2K elastic-net specification and its fixed `tau = 0.15` transfer rule to remain tied to the SYN-DIGITS implementation. Gate behavior was examined on the Wave-4 tuning fold and separate MegaStudy outcomes; the 61-anchor holdout was opened once to evaluate the ungated calibration specification. No Silicon human outcome was used for model selection, calibration, or gating.

## K · Reproducibility and frozen artifacts

- **K.1 Code and materials:** Public code and derived matrices are in this repository. `code/calib/reproduce_submission.py` regenerates the 221 elastic-net calibrations from the public matrices and verifies both submitted files; `code/calib/poststratification.py` constructs and audits the fixed 40-cell reweighting factors. The original response-to-target reconstruction remains in `code/calib/production.py` and uses the escrowed run archive. The SYN-DIGITS calibration implementation is vendored under its MIT license in `code/syn-digits/`.
- **K.2 Raw logs †:** The adopted provider-response files, session-completeness ledger, Wave-4 calibration anchors, individual target matrices, demographic weighting inputs, diagnostics, and reproduction code are under restricted access at [10.5281/zenodo.22168892](https://doi.org/10.5281/zenodo.22168892). The public `artifacts/` directory contains the aligned anchor matrices, raw and calibrated 1,921 × 221 target matrices, target grid, respondent weights, and weighting audit needed to reproduce the submitted means.
- **K.3 Resources:** The calibration fit 221 elastic-net target models on 1,921 twins using one workstation. Calibration itself used no API calls.

## L · Disclosure class

**Class B (escrowed).** Prediction files, calibration code, anchors, target matrices, and audits are public; the adopted provider responses and supporting reconstruction evidence are escrowed at [10.5281/zenodo.22168892](https://doi.org/10.5281/zenodo.22168892).

## References

Fan, Grace Jiarui, Chengpiao Huang, Tianyi Peng, Kaizheng Wang, and Yuhang Wu (2026), “SYN-DIGITS: A Synthetic Control Framework for Calibrated Digital Twin Simulation,” arXiv:2604.07513. [https://doi.org/10.48550/arXiv.2604.07513](https://doi.org/10.48550/arXiv.2604.07513).

Toubia, Olivier, George Z. Gui, Tianyi Peng, Daniel J. Merlau, Ang Li, and Haozhe Chen (2025), “Database Report: Twin-2K-500,” *Marketing Science*, 44 (6), 1446–1455. [https://doi.org/10.1287/mksc.2025.0262](https://doi.org/10.1287/mksc.2025.0262).
