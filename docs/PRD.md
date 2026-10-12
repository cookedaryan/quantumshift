# Product requirements: QuantumShift

Source: project plan Sections 3, 9 and 12. Success targets are set only where the plan gives one; everything else is set after the baseline run (see [EVALUATION.md](EVALUATION.md)).

## Problem

Organisations must replace RSA and ECC before quantum computers arrive, but most do not know where these algorithms are used. Cryptography is scattered across application code, third-party libraries, configuration files, certificates and compiled programs bought from vendors. Building this list by hand (a cryptographic inventory) is slow and error-prone. Even with a list, teams lack clear, standards-based guidance on what to replace each item with and what it will cost in performance.

## Users

Security and platform engineers at an organisation migrating to post-quantum cryptography. They need an inventory, a priority order, and a reviewable replacement plan for each finding. Generated plans are advice for engineers to review, never changes applied automatically.

## Objectives

| ID | Objective | How we will know it is achieved |
|---|---|---|
| O1 | Detect cryptography in source code (Java, Python, C++), configuration files, certificates and compiled binaries. | All crypto uses in the demo system are found. |
| O2 | Use ML to reduce false alarms compared with simple pattern matching. | Higher precision than Semgrep rules at similar recall. |
| O3 | Find cryptography in stripped binaries with no symbol names. | Detects the demo ATM firmware's curve; beats findcrypt-yara on our test set. |
| O4 | Rank findings by quantum risk. | Risk scores follow Mosca's rule. |
| O5 | Produce a standard cryptographic inventory. | Output validates as CycloneDX 1.6 CBOM. |
| O6 | Generate cited, hardware-aware migration plans with RAG. | Citations point to real sections; every number matches our benchmark data. |
| O7 | Prove one migration works end to end. | A browser connects to the demo server with hybrid post-quantum TLS. |

## Scope

| In scope | Out of scope |
|---|---|
| Java, Python and C++ source code | Every programming language |
| nginx, Apache, SSH and Spring configuration files | Hardware security modules and smart cards |
| X.509 certificates and Java/PKCS#12 keystores | Breaking or attacking any encryption |
| Linux and Windows binaries (ELF and PE) | Automatically rewriting a whole codebase |
| Benchmarks on a laptop and a Raspberry Pi | Scanning systems we do not own |

Extra languages only if time remains.

## Success metrics

| Metric | Target | Baseline it must beat |
|---|---|---|
| Source-code detection precision and recall | Higher precision at similar recall; exact numbers set after baseline run | Semgrep rules alone; XGBoost model |
| Binary detection | Beats the baseline on our test set; detects the demo ATM firmware's curve | findcrypt-yara rules |
| Whole tool | Reported against the industry reference | IBM sonar-cryptography plugin (Java only) |
| Numeric accuracy of plans | 100% of numbers match the benchmark table | n/a |
| Citation accuracy | Share of citations that truly support the claim, checked by hand on a sample; target set after first run | n/a |
| Faithfulness (RAGAS) | Set after first run | n/a |
| Expert review | A faculty member in cryptography or security rates 20 plans | n/a |
| Benchmark reproducibility | Repeated runs on the same device agree within a few percent | n/a |

Full definitions and result tables: [EVALUATION.md](EVALUATION.md).

## Risks

| Risk | Impact | How we handle it |
|---|---|---|
| Labelling takes too long | ML model is weak or late | Start in week 1; use rules to pre-label and only correct; use public benchmarks first |
| LLM invents facts or citations | Wrong advice | Numbers come only from the database; guards verify every number and citation |
| liboqs or OpenSSL 3.5 fails to build on the Pi | No ARM benchmarks | Set up the Pi in phase 1; use a 64-bit OS; fall back to Docker images |
| Three languages are hard to integrate | Parts work alone but not together | Shared `.proto` file from week 1; thin end-to-end path in phase 2 |
| Scope grows | Nothing is finished | Stick to the scope table; extra languages only if time remains |
| Standards change | Out-of-date advice | Re-check NIST pages before the final demo; re-index the RAG corpus |
| Live demo fails | Poor evaluation | Backup video, pre-loaded results, rehearsals |

## Ethics and legal boundaries

- The tool finds and plans; it never attacks or breaks encryption.
- Network scanning is done only on systems we own, such as QuantumBank and our own servers.
- QuantumBank is fictional and contains no real personal data or real private keys.
- Code mined from GitHub is used only from projects whose licences permit it, and only for research.
