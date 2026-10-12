# classifier/ (Python 3.12)

Reads source code (Java, Python, C++) and decides which snippets really use cryptography.

- Two stages. Stage 1: tree-sitter builds syntax trees and rules extract every candidate (aim for high recall, accept false alarms). Stage 2: a fine-tuned UniXcoder model labels algorithm, purpose and quantum-vulnerable (aim for precision).
- Baselines to beat: Semgrep rules alone, and an XGBoost model on hand-made features.
- Data: CryptoAPI-Bench, CamBench, and mined open-source Python/C++ snippets. Check each licence and record it in docs/EVALUATION.md.
- Split 70% train, 15% validation, 15% test. The test set is used only for final results, never for tuning.
- Labelling guideline comes first; two people label a shared sample to check agreement.
- Decoys must never be flagged as quantum-vulnerable (see demo/quantumbank/README.md). Names like `rsaCount` and commented-out code are not crypto at all; SHA-256 and AES-256 are crypto but quantum-safe. Crypto inside unit-test files is also a planted decoy; how it is labelled is for the labelling guideline to define.
- Served over gRPC using proto/findings.proto. Never change the proto here; open an ADR.
- Tests: `pytest -q`. Report precision, recall and F1 with scikit-learn, and log them in docs/EVALUATION.md.
