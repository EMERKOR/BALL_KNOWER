# Research-led roadmap for Ball Knower

Status: proposed replacement work sequence, 2026-09-27. [Published architecture reference](PUBLISHED_NFL_MODEL_ARCHITECTURES.md) is the evidence register. This roadmap supersedes the sequencing in root `ROADMAP.md`, `BALL_KNOWER_V3_MASTER_ROADMAP.md`, and `ball_knower_v3/CHALLENGER_RESEARCH_ROADMAP.md`; those files remain historical implementation records. No model architecture or weight is selected by this document.

The goal is a basic, auditable forecasting baseline with optional feature layers. Reproduce working published approaches first, evaluate them on the same timestamped data, then choose what to retain. Betting and player props require additional quote/availability evidence. Ball Knower has no working end-to-end forecasting model. Existing code and reports are unfinished development material, not a comparison baseline. First get a published reference model running end to end.

## Work queue and acceptance gates

| ID | Work item | Concrete output / gate | Dependencies |
| --- | --- | --- | --- |
| R1 | Source and license ledger | Pin upstream commits and licenses for 538 Elo, schwill Elo, nfelo and nfelotranslation, melo, kshreyan, nfelounits, TMoiseyenko, Lopez papers; record target, defaults, fit objective, training/evaluation chronology, published metric and missing inputs. Link exact code. Separate author claims from reproduced results. | None |
| R2 | Target and forecast-time contract | Write a one-page table for win, margin, total, spread/total prices and each prop: prediction unit, outcome, integer/push handling, UTC issuance cutoff, known-at time, and scoring. Decide earliest target/cutoff based on available data, recording this as a project decision. | R1 |
| R3 | Data availability proof | For nflverse and any QB/injury/starter/weather source, archive sample source revisions and availability timestamps for multiple weeks; compare historical snapshots to current final data. Define game/team/player join keys, provenance and missingness report. If a feature lacks historical availability, exclude it from historical backtests. | R2 |
| R4 | Historical market quote proof | Obtain real provider samples at the proposed cutoff for several weeks/books and each target market, including an event-level prop sample. Archive request time, returned snapshot time, line, odds, book, market and event ID; quantify coverage, cost and terms. Gate market-comparison or betting work on successful audit. | R2 |
| R5 | Shared replay and scoring harness | One chronological origin schedule with training-only transformations/tuning, immutable pregame feature snapshots, identical eligible games, logged exclusions and versioned predictions. Add log loss, Brier, calibration, CRPS/distribution log score, coverage, exact integer mass; compare to a prevalence/league, simple Elo, and timestamped market baseline when available. Tests detect future-row leakage and later-file revision sensitivity. | R2, R3; market baseline depends on R4 |
| R6 | Get a published winner model running | Run pinned 538 Elo on its own published example data and reproduce its output end to end. Record exact code/data versions, inputs, predictions and discrepancies. Then run schwill Elo. Common held-out scores follow once R5 exists. Do not copy weights into a changed formula. | R1 for publisher fixture; R5 for common replay |
| R7 | Reproduce published distribution approaches | Run pinned melo margin and total method and kshreyan Ridge margin method; document changes needed for integer scoring/push, published defaults versus time-safe refits, and held-out distribution diagnostics. Include a simple empirical outcome distribution only as a declared control. | R1, R5 |
| R8 | Trace nfelo and inventory unfinished Ball Knower code | Trace nfelo inputs, prior preparation, market regression, open versus close paths, translation and optimization objective; reproduce available paths and label missing proprietary inputs. Record which Ball Knower pieces exist and what is missing for an end-to-end forecast. There is no v3 score comparison until a functional model exists. | R1 |
| R9 | Test optional football feature layers | Add one source-justified family at a time (e.g. EPA, rest, QB, units) only after R3 proves as-of data. Fit weights on earlier origins using the upstream objective or a preregistered proper scoring objective. Save ablations, coefficient stability, calibration and held-out delta with uncertainty; remove layers without credible gain. | R3, R5, R6–R8 |
| R10 | Player and prop architecture reproduction | Pin kshreyan direct-stat Ridge and TMoiseyenko role/share distribution implementations. Establish game/player/stat identity, snaps/availability, integer/count distributions and separate volume/share calibration. Score out-of-sample by stat; only compare to actual prop lines after R4 validates timestamped quote coverage. | R3–R5, R1 |
| R11 | Select a maintainable reference and extension interface | Write a decision record containing comparable score table, data costs, missingness, uncertainty, calibration, run time and replication status. Select the simplest defensible target-specific baseline(s) and an explicit feature input/output contract; archive alternatives. This decision follows results, not this roadmap. | R6–R10 as applicable |
| R12 | Operational and wagering gate | Version forecasts and quote snapshots, calculate no-vig probability/EV with push and settlement rules, log offered/executed prices and limits, run prospective paper tracking before any strategy claim. Report net results with uncertainty; no historical ROI from final or closing lines used as though known earlier. | R4, R5, R11 |

## Additional source investigations

[Practitioner and commercial leads](PRACTITIONER_DATA_MARKET_LEADS.md) records SharpStack's market-derived pricing and operations, Fantasy Points Data Suite's charted feature inventory, and Circa/Pinnacle first-person accounts. R1 must capture their disclosed methods and omissions; R3 must test Fantasy Points' historical timestamp and licensing claims; R4 must test SharpStack's quote/algorithm transparency and available exports. R8/R9/R12 can use those findings only after their gates. No proprietary fair-odds claim or practitioner account is a reproduced architecture.

## Immediate order

Start R6 now: run the published 538 reference on its own example data and show the predictions it makes. R1 pins the source version; R2 and R3 check the data needed for a common replay. R4 can run alongside R3. R5 enables comparable historical scores. R7 then covers margin and total distributions. R8 traces nfelo and records the gaps in Ball Knower. Only a functioning implementation can later enter the same-game evaluation. R9 and R10 are conditional extensions. R11 is the first architecture choice; R12 is a separate market/operations claim.

## Definition of done for any model comparison

Every result names a pinned code revision, source/data revision, training and test seasons, forecast UTC cutoff, sample count and exclusions, hyperparameter search space/objective, proper score, calibration, comparison forecast, uncertainty, and whether lines were actually observable then. Preserve predictions and reproduce from a fresh checkout. A reported upstream percentage, a same-sample fit, or a handful of successful wagers does not pass this gate.

## Decision log and change control

- **Accepted process**: use published inspectable implementations as reference; reproduce before adopting architecture or coefficients. User direction, 2026-09-27.
- **Open**: first target and cutoff, data vendors, baseline winner, model distribution, weight values and prop family. Resolve with the gates above.
- Existing Phase 3C reports and contracts describe unfinished code. They are not forecasts from a functional model and do not dictate the project sequence. Any change to target/cutoff or source history requires rerunning the common replay.
