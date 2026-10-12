# Evaluation

Every number in the report comes from a script you ran and logged here, never from model text. Results are empty until the team runs the evaluations. Fill in the date and commit with each result.

## Datasets and split

- Source-code labels: CryptoAPI-Bench and CamBench (Java), plus Python and C++ snippets mined from open-source projects, labelled by the team with a written guideline. Two people label a sample of the same snippets to check agreement.
- Split: 70% training, 15% validation (for tuning), 15% test. The test set is touched only for final results.
- Results are reported on the held-out test set and on QuantumBank (ground-truth file in `demo/quantumbank/`).

## Baselines

| Our part | Baseline it must beat |
|---|---|
| Source-code detection (rules + fine-tuned UniXcoder) | Semgrep rules alone; XGBoost model |
| Binary detection (C++ scanner) | findcrypt-yara rules |
| Whole tool | IBM sonar-cryptography plugin (Java only) |

## Metrics and results

| Metric | Definition | Result | Date / commit |
|---|---|---|---|
| Precision | Of everything flagged, the fraction that were real problems | | |
| Recall | Of all real problems, the fraction found | | |
| F1 | Harmonic mean of precision and recall | | |
| Citation accuracy | Share of citations that point to a passage that truly supports the claim, checked by hand on a sample | | |
| Faithfulness | RAGAS score of whether each sentence is supported by retrieved passages | | |
| Numeric accuracy | Share of numbers in plans that exactly match the benchmark table (target 100%) | | |
| Expert review | Ratings of 20 plans by a faculty member in cryptography or security | | |
| Benchmark reproducibility | Agreement of repeated runs on the same device | | |
| Scan time | Duration of a full QuantumBank scan | | |

### Per-baseline comparison

| Part | System | Precision | Recall | F1 | Test set | Date / commit |
|---|---|---|---|---|---|---|
| Source code | Semgrep rules | | | | | |
| Source code | XGBoost | | | | | |
| Source code | Rules + UniXcoder | | | | | |
| Binaries | findcrypt-yara | | | | | |
| Binaries | C++ scanner | | | | | |
| Whole tool | sonar-cryptography | | | | | |
| Whole tool | QuantumShift | | | | | |

## Benchmark runs

Each benchmark result is tagged with a hardware profile (laptop x86, Raspberry Pi ARM) and reports median and p95 over thousands of runs after discarded warm-up runs.

| Algorithm | Operation | Hardware profile | Median | p95 | Date / commit |
|---|---|---|---|---|---|
| | | | | | |

## Licences

Check dataset and model licences before use.

| Dataset or model | Licence | Checked by | Date |
|---|---|---|---|
| | | | |
