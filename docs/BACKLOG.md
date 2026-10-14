# Issue backlog (draft)

Draft split of the project plan into GitHub issues, following playbook prompt 4. **Nothing here is decided until the team reviews it.** Copy each row into a GitHub issue using the feature template. Create the milestones first.

## How to read this

- **Milestones** are the plan's five build phases (Section 11), with the fourth split into 4a and 4b. The playbook suggests four milestones; the plan has five phases, so this follows the plan. The plan gives no end date, so the **suggested due dates assume a 20-week schedule from 12 Oct 2026** (see [TIMELINE.md](TIMELINE.md)). Shift them once the team fixes its real deadline.
- **Hardware-aware work is deliberately late** (milestone 4b). Milestones 1 to 4a only need a `laptop` hardware profile; the only early hardware task is a one-day Pi smoke test (M1-09).
- **Owner** is a role from plan Section 10. TL is the tech lead (not yet assigned). "All" means one issue per member.

| Owner | Role | CODEOWNERS handle |
|---|---|---|
| R1 | C++ binary scanner | `@member1-cpp-scanner` |
| R2 | C++ benchmarks, Pi, hybrid TLS demo | `@member2-cpp-bench` |
| R3 | Python dataset, labelling, tree-sitter | `@member3-py-data` |
| R4 | Python model training, evaluation, inference service | `@member4-py-model` |
| R5 | Java orchestrator, parsers, risk scoring, CBOM | `@member5-java-core` |
| R6 | Java RAG pipeline, guards, React dashboard | `@member6-java-rag` |

- **Size:** S is up to 1 day, M about 2 days, L about 3 days. Nothing is bigger than 3 days: if an L turns out larger, split it.
- **Priority:** Milestone 2 is the most important (plan Section 11). A simple pipeline that works end to end is worth more than one brilliant part that is not connected.
- Numbers marked *proposed* are suggestions for the team to confirm. Everything else comes from the plan.

## Milestone 1: Foundations

Suggested due: Sun 25 Oct 2026 (weeks 1-2). Goal: everyone can build and run the skeleton.

| ID | Title | Owner | Acceptance criteria | Depends on | Size |
|---|---|---|---|---|---|
| M1-01 | GitHub setup and placeholders | TL | Repo, collaborators, branch protection, labels, `ANTHROPIC_API_KEY`, pinned Daily log issue done; real handles in CODEOWNERS; journal workflow run once by hand | none | M |
| M1-02 | Define gRPC services and message shapes | R5 | Request and response messages for `ScanBinary` and the classifier call added to `proto/`; code generates for C++, Java and Python; `docs/API.md` updated; ADR merged | none | M |
| M1-03 | Docker Compose skeleton | R5 | `docker compose up` starts Postgres with pgvector and placeholder containers for scanner, classifier, server and web; `.env.example` covers every variable | M1-02 | M |
| M1-04 | Java server skeleton | R5 | Spring Boot 3 / Java 21 project in `server/`; `mvnw` is executable in git (`git update-index --chmod=+x`); `./mvnw verify` passes; health endpoint; CI java job runs green | M1-01 | M |
| M1-05 | Skeleton end to end | R5 | Orchestrator calls the scanner and classifier stubs over gRPC and returns one merged dummy finding; integration test passes | M1-02, M1-04, M1-06, M1-07 | S |
| M1-06 | Scanner skeleton | R1 | CMake project with LIEF and libyara linked; gRPC `ScanBinary` stub returns a fixed Finding; ctest passes; CI cpp job green | M1-02 | M |
| M1-07 | Classifier skeleton | R4 | `pyproject.toml` with a `dev` extra (ruff, pytest); gRPC server stub returns fixed labels; pytest passes; CI python job green | M1-02 | S |
| M1-08 | Benchmark skeleton | R2 | CMake project with liboqs-cpp and OpenSSL; ML-KEM-768 key generation timed with discarded warm-up runs; prints median and p95; ctest passes | M1-01 | M |
| M1-09 | Raspberry Pi smoke test | R2 | 64-bit OS; liboqs and OpenSSL 3.5 build on the Pi; the M1-08 benchmark runs once on it; build steps written in `bench/README.md`; Docker-image fallback documented if the build fails. Full Pi benchmarking waits for M4b | M1-08 | S |
| M1-10 | Labelling guideline | R3 | Written guide for the three labels (algorithm, purpose, quantum-vulnerable) with decoy rules (comments, variable names, tests) and 20 worked examples; R4 has reviewed it | none | M |
| M1-11 | Collect public datasets | R3 | CryptoAPI-Bench and CamBench in `data/raw/` (gitignored), converted to one snippet format; licences recorded in `docs/EVALUATION.md` | none | M |
| M1-12 | Start labelling and agreement check | R3 + R4 | First batch labelled (*proposed* 300 snippets); R3 and R4 label the same sample independently and agreement is reported | M1-10, M1-11 | L |
| M1-13 | Dashboard skeleton | R6 | React + Vite app in `web/` builds; page shows the server health response; runs under compose | M1-03, M1-04 | S |
| M1-14 | RAG corpus list | R6 | File listing every source document (FIPS 203/204/205, NIST IR 8547, SP 800-208, CNSA 2.0, IETF hybrid TLS drafts, Bouncy Castle / OpenSSL / JDK PQC docs) with URL, version and licence; PDFs saved to `data/raw/` | none | M |

## Milestone 2: Thin end-to-end path

Suggested due: Sun 29 Nov 2026 (weeks 3-7). Goal: one finding travels from input to dashboard. Rule-based detection, simple risk score, CBOM, basic RAG plan, minimal dashboard.

| ID | Title | Owner | Acceptance criteria | Depends on | Size |
|---|---|---|---|---|---|
| M2-01 | tree-sitter extraction for Java | R3 | Given a Java file, returns candidates (API call, algorithm string, import) with file and line; tests cover comment and variable-name cases | M1-07 | L |
| M2-02 | tree-sitter extraction for Python and C++ | R3 | Same as M2-01 for Python and C++ files | M1-07 | L |
| M2-03 | Rule-based classifier | R4 | Maps candidates to Findings (algorithm, purpose, vulnerable) with a lookup table; served over gRPC; tests for the plan 5.3 examples (RSA getInstance, `rsaCount`, commented-out RSA, `hashlib.sha256`) | M2-01 | M |
| M2-04 | Scanner: imports and strings | R1 | LIEF parses ELF files; reports crypto imports (for example `RSA_public_encrypt`) and library version strings as Findings; ctest uses a small test ELF | M1-06 | L |
| M2-05 | nginx config parser | R5 | Parses `nginx.conf` and flags `ssl_certificate` key type, `ssl_ciphers` and `ssl_ecdh_curve` as in plan 5.4; unit tests | M1-04 | M |
| M2-06 | Certificate and keystore parser | R5 | Bouncy Castle reads `.pem`, `.crt`, `.jks`, `.p12`: key type, size and expiry become Findings; test keystores are fake | M1-04 | M |
| M2-07 | Orchestrator job runner and merge | R5 | A scan job takes a path or zip, calls the finders in parallel, merges duplicates, stores results in Postgres; merge tests | M1-05, M2-03, M2-04, M2-05, M2-06 | L |
| M2-08 | Simple risk score | R5 | Asset details file (data lifetime, migration time, internet exposure); Mosca x + y > z with configurable z; P1 to P4; tests with the plan's two examples (medical records critical, one-time codes low) | M2-07 | M |
| M2-09 | CBOM export | R5 | CycloneDX 1.6 through cyclonedx-core-java; a test validates the output against the schema; ECDSA-P256 shows `nistQuantumSecurityLevel` 0 | M2-07 | M |
| M2-10 | Benchmark core operations | R2 | Key generation, encapsulation, decapsulation, sign and verify for the algorithms in plan 5.7 on the laptop; results written to Postgres in a table with a `hardware_profile` column (only `laptop` is populated for now) | M1-08, M1-03 | L |
| M2-11 | Benchmark size check | R2 | Test asserts public key, ciphertext and signature sizes equal the plan 5.7 table | M2-10 | S |
| M2-12 | RAG ingestion v1 | R6 | Tika parses the corpus; naive chunks embedded into pgvector; a query returns top-k chunks | M1-14, M1-03 | L |
| M2-13 | Basic RAG plan | R6 | For one finding type, Spring AI with Ollama writes a plan from retrieved passages; checked by hand on 3 findings | M2-12 | L |
| M2-14 | Minimal dashboard | R6 | Upload page and an inventory table fed by the server API | M1-13, M2-07 | M |
| M2-15 | QuantumBank: banking API | R5 | Spring Boot app with RSA-2048 key wrapping and ES256 login tokens (plan 8.1); fake keys only; builds | M1-04 | M |
| M2-16 | QuantumBank: keystore | R5 | `.jks` with several RSA key pairs, generated for the demo and clearly fake | M2-15 | S |
| M2-17 | QuantumBank: notification service | R3 | Python service doing ECDH with the `cryptography` library | M1-07 | S |
| M2-18 | QuantumBank: ATM firmware | R1 | C++ program statically linked with mbedTLS, stripped, verifying updates with ECDSA; binary stored in `demo/quantumbank/` | M1-06 | L |
| M2-19 | QuantumBank: nginx config | R6 | ECDHE-RSA cipher suites and an RSA-2048 certificate (fake) | none | S |
| M2-20 | QuantumBank: decoys | R4 | SHA-256, AES-256, a variable named `rsaCount`, a class named `EccReport`, commented-out RSA and crypto in unit tests, spread across Java, Python and C++ | M2-15, M2-17 | M |
| M2-21 | Ground-truth file | R4 | Machine-readable list of every real crypto use and decoy with location; format documented; checked against M2-15 to M2-20 | M2-15, M2-16, M2-17, M2-18, M2-19, M2-20 | M |
| M2-22 | Thin-path demo | R5 | On `docker compose up`: scan QuantumBank, see the inventory, a risk priority, a CBOM download and one plan; screen recording saved | M2-07, M2-08, M2-09, M2-13, M2-14, M2-21 | M |

## Milestone 3: Intelligence

Suggested due: Sun 10 Jan 2027 (weeks 8-13; weeks 11-12 are lighter for exams and holidays). Goal: the clever parts work. Fine-tuned classifier, binary scanner, section-aware RAG with reranking, guards.

| ID | Title | Owner | Acceptance criteria | Depends on | Size |
|---|---|---|---|---|---|
| M3-01 | Finish labelling and freeze splits | R3 | Labelled set reaches the agreed size (plan target about 2,000 to 3,000 snippets); 70/15/15 split files frozen; test set locked | M1-12 | L |
| M3-02 | Stage-1 recall measurement | R3 | Script reports candidate recall on the labelled set; misses analysed and extraction rules fixed until the agreed recall is met | M3-01, M2-01, M2-02 | M |
| M3-03 | Semgrep baseline | R4 | Semgrep crypto rules run on the test set; precision, recall and F1 recorded in `docs/EVALUATION.md` | M3-01 | M |
| M3-04 | XGBoost baseline | R4 | Hand-made features; trained on train, tuned on validation; test result logged | M3-01 | M |
| M3-05 | Fine-tune UniXcoder | R4 | Training script and config in the repo; reproducible run; validation metrics logged; weights not committed | M3-01 | L |
| M3-06 | Evaluation script | R4 | One script computes precision, recall and F1 per label (scikit-learn) for all systems and writes a table | M3-03, M3-04, M3-05 | M |
| M3-07 | Model in the gRPC service | R4 | Service runs rules then the model, returns Findings with confidence; latency measured on QuantumBank; tests | M3-05, M2-03 | M |
| M3-08 | Scanner: constant signatures | R1 | YARA rules for the plan 5.2 fingerprints (AES S-box, SHA-256 constant, P-256 prime, RSA identifier); each rule tested on a known binary | M2-04 | L |
| M3-09 | Scanner: endianness | R1 | Rules match big- and little-endian 32- and 64-bit layouts; a test per layout | M3-08 | M |
| M3-10 | Stripped ATM firmware detection | R1 | Scanner reports the P-256 prime and the `mbed TLS 2.28` string from M2-18 with offsets and confidence; a test asserts both | M3-09, M2-18 | M |
| M3-11 | Binary baseline comparison | R1 | findcrypt-yara run on the same binary test set; results table in `docs/EVALUATION.md` | M3-10 | L |
| M3-12 | Section-aware chunking | R6 | Chunks carry document and section number (for example "FIPS 204, Section 4"); a test checks 10 sampled chunks | M2-12 | L |
| M3-13 | Reranker | R6 | Retrieved chunks are reranked and trimmed; hit rate before and after logged on a 20-query retrieval test set | M3-12 | M |
| M3-14 | Benchmark lookup for the planner | R6 | Planner fetches benchmark rows by algorithm and a hardware-profile parameter (default `laptop`) from Postgres and gives them to the LLM as a table; the LLM never generates numbers | M2-10, M2-13 | M |
| M3-15 | Structured plan format | R6 | Plan has finding, priority, recommendation, why (citations), performance and phase (plan 5.8); validated against a JSON schema | M2-13 | M |
| M3-16 | Guards | R6 | Numeric guard (every number matches the table) and citation guard (every citation was retrieved); failures flagged for a human; tests with deliberately wrong output | M3-14, M3-15 | L |
| M3-17 | Full risk factors | R5 | Score combines vulnerability, data lifetime, exposure and purpose (key exchange ranks above signature); table-driven tests | M2-08 | M |
| M3-18 | Live scan progress | R5 | Server pushes per-finder progress over WebSocket; dashboard shows progress bars | M2-07, M2-14 | M |
| M3-19 | Dashboard filters and risk heatmap | R6 | Filter by algorithm, file, priority and finder; risk heatmap of systems and data types | M2-14, M3-17 | L |

## Milestone 4a: Core measurement

Suggested due: Sun 31 Jan 2027 (weeks 14-16). Goal: the system is feature-complete on the `laptop` profile: real benchmark numbers, hybrid TLS, and the Bouncy Castle migration demo.

| ID | Title | Owner | Acceptance criteria | Depends on | Size |
|---|---|---|---|---|---|
| M4-01 | Full benchmark matrix on the laptop | R2 | Every algorithm in the plan 5.7 table, thousands of runs after warm-up, median and p95 stored | M2-10 | L |
| M4-02 | Peak memory | R2 | Peak memory per operation recorded alongside speed | M4-01 | S |
| M4-04 | TLS handshake benchmark (laptop) | R2 | Time and bytes sent for X25519 versus X25519MLKEM768 with OpenSSL 3.5 on the laptop profile | M1-09 | M |
| M4-05 | Hybrid TLS demo site | R2 + R6 | nginx with OpenSSL 3.5 serves QuantumBank using `ssl_ecdh_curve X25519MLKEM768:X25519`; Chrome connects; a Wireshark capture shows group 0x11EC | M2-19 | L |
| M4-07 | Banking API ML-KEM migration | R5 | Customer key wrapping uses ML-KEM hybrid with X25519 through Bouncy Castle (plan 6 and 8.3); automated tests pass | M2-15 | L |
| M4-09 | Plan view with clickable citations | R6 | Clicking a citation opens the standard at that section | M3-12, M3-15 | M |
| M4-10 | CBOM download in the UI | R6 | Download button; the downloaded file validates against the schema | M2-09, M2-14 | S |
| M4-11 | Full-pipeline run | R5 | A QuantumBank scan covers binaries, ML, configs, risk and plans with no manual steps, on the `laptop` profile | M3-07, M3-10, M3-16, M3-17 | M |

## Milestone 4b: Hardware-aware block

Suggested due: Sun 14 Feb 2027 (weeks 17-18). Goal: add the Pi profile and hardware-aware plans, then freeze the planner. If the Pi slips, this whole block can fall back to "laptop only" (see [TIMELINE.md](TIMELINE.md)).

| ID | Title | Owner | Acceptance criteria | Depends on | Size |
|---|---|---|---|---|---|
| M4-03 | Benchmarks on the Pi | R2 | Full matrix on the Pi stored as its own profile; two repeat runs compared and the spread recorded; a short mode that can run live in the demo | M1-09, M4-01 | M |
| M4-12 | TLS handshake benchmark (Pi) | R2 | Same measurements as M4-04, stored under the Pi profile | M4-04, M1-09 | M |
| M4-06 | Hardware-aware plans | R6 | Switching profile from server to Pi changes the recommendation as in plan 5.8 (hybrid ML-KEM for the server key exchange; avoid SLH-DSA on the Pi); one test per case | M3-14, M4-03 | M |
| M4-08 | Dashboard benchmark charts | R2 | Recharts comparing old and new algorithms with a profile switch, read from the server API. *Moved from R6 to balance load; team to confirm* | M4-01, M4-03, M2-14 | M |
| M4-13 | Planner freeze | R6 | Planner, prompts, guards and hardware rules tagged at one commit and the tag recorded in `docs/EVALUATION.md`; later planner changes need team agreement and a rerun of M5-04 | M4-06, M3-16 | S |

## Milestone 5: Evaluation and polish

Suggested due: Sun 28 Feb 2027 (weeks 19-20). Goal: ready to present. QuantumBank complete, accuracy results, report, rehearsed demo and backup video.

| ID | Title | Owner | Acceptance criteria | Depends on | Size |
|---|---|---|---|---|---|
| M5-01 | Final source-code evaluation | R4 | Semgrep, XGBoost and rules plus UniXcoder compared on the test set and QuantumBank; results with commit hash in `docs/EVALUATION.md` | M3-07, M3-06 | M |
| M5-02 | Final binary evaluation | R1 | Scanner versus findcrypt-yara on the test set and the ATM firmware, logged | M3-11 | S |
| M5-03 | Whole-tool comparison | R5 | IBM sonar-cryptography run on the QuantumBank Java code; QuantumShift versus it against the ground truth | M4-11, M2-21 | M |
| M5-04 | RAG evaluation | R6 | RAGAS faithfulness; citation accuracy hand-checked on a sample; numeric accuracy script (target 100%); run only after the planner freeze | M3-16, M4-13 | L |
| M5-05 | Expert review | TL | A faculty member in cryptography or security rates 20 plans; ratings stored | M5-04 | M |
| M5-06 | Accuracy panel | R6 | Dashboard compares findings against the ground truth and shows precision and recall | M2-21, M3-07 | M |
| M5-07 | PDF report export | R6 | Dashboard exports a PDF report. *Cut candidate* | M4-09 | L |
| M5-08 | Performance numbers | R5 | Full-scan time and benchmark reproducibility recorded in `docs/EVALUATION.md` | M4-11, M4-03 | S |
| M5-09 | Re-check the standards | R6 | NIST pages re-checked; corpus re-indexed if anything changed; noted in `docs/EVALUATION.md` | M3-12 | S |
| M5-10 | Licence audit | R3 | Every dataset and model has a licence row in `docs/EVALUATION.md`; licences of mined GitHub code confirmed | M3-01 | S |
| M5-11 | Report section (one per member) | All | Each member writes their module's section of the report using numbers from `docs/EVALUATION.md` only | M5-01, M5-02, M5-04 | S |
| M5-12 | Slides and demo script | TL | 12-minute script following plan 8.2 with timings per step | M4-11 | M |
| M5-13 | Backup video and rehearsals | All | Screen recording of the full demo; pre-computed scan results loaded; Pi results saved as a chart; three timed rehearsals | M5-12 | M |
| M5-14 | Viva preparation (one per member) | All | Each member runs playbook prompt 10 on their module and can explain every PR they merged | M5-13 | S |
| M5-15 | Real-repo spot check | R3 | Scan 3 to 5 public open-source projects whose licences allow it; hand-check every finding; report precision on them separately from QuantumBank (accuracy on your own planted system looks better than reality) | M3-07 | M |

## Notes for the team

- **Load balance.** Adding up sizes (S = 1, M = 2, L = 3 days) gives roughly these totals per owner, before the shared "All" issues (about 4 days each): R6 45, R5 35, R4 23, R2 22, R3 22, R1 19. R6 (RAG pipeline, guards, dashboard, nginx) and R5 (orchestrator, parsers, risk, CBOM, banking API) carry far more than the others. The plan assigns these roles, so the table keeps them, but the team should rebalance before starting. Candidates:
  - M1-14 (RAG corpus list) to R3, who already collects datasets.
  - M2-19 (QuantumBank nginx config) to R2, who does the hybrid TLS demo on nginx in M4-05.
  - Dashboard issues (M3-19, M5-06, M5-07) to whoever on the team knows React best.
  - M4-08 (benchmark charts) is already moved to R2 in the table; confirm it.
- **If the Pi slips:** milestone 4b can fall back to laptop-only. Drop M4-03, M4-12 and M4-06; keep M4-13 (freeze) and M4-08 with a single profile; M5-08 then records laptop reproducibility only. Say clearly in the report that the hardware-aware part is partial.
- **Cut order if time runs short:** M5-07 (PDF export), the Windows PE support (not listed here), the Cytoscape dependency graph (not listed here), the sonar-cryptography comparison (M5-03), then the expert review (M5-05). Keep the core story: detect, filter with ML, prioritise, plan with RAG backed by real benchmarks.
- **Each issue** gets an owner and a branch `feature/<number>-<slug>`. M1-01 is the only one that needs to start before the repo exists.
