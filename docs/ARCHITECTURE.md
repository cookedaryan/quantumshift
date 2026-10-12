# Architecture

Source: project plan Sections 4, 5 and 7. The contract between services is [API.md](API.md).

## Flow

A user uploads a project. Three finders search it in parallel: the C++ binary scanner looks inside compiled programs, the Python classifier reads source code, and the Java config parser reads settings files and certificates. Their findings go to the Java orchestrator, which merges them into one inventory, scores the risk of each item and produces a CycloneDX 1.6 CBOM. The Java RAG planner then writes a replacement plan for each finding, using the NIST standards and the performance numbers measured by the C++ PQC benchmark suite. The React dashboard shows everything.

```
Inputs (code, binaries, configs, certs, asset details)
   |
   +--> C++ binary scanner ------gRPC---+
   +--> Python classifier --------gRPC--+--> Java orchestrator --> inventory, risk, CBOM
   +--> Java config parser -------------+          |                       |
                                                   v                       v
C++ PQC benchmarks --> PostgreSQL <-------- Java RAG planner         React dashboard
                       (pgvector)          (NIST rules + benchmarks)  (plan, CBOM, report)
```

## Components

| Component | Folder | Language | Owner role (plan Section 10) |
|---|---|---|---|
| C++ binary scanner: LIEF, YARA rules, endianness handling. Finds constants, imported symbols and strings, including in stripped binaries. | `scanner/` | C++17 | 1 |
| C++ PQC benchmark suite: speed, sizes, memory and TLS handshake for new and old algorithms (liboqs, OpenSSL 3.5), tagged by hardware profile. Hybrid TLS demo setup. | `bench/` | C++17 | 2 |
| Python classifier: tree-sitter candidate extraction (high recall), then a fine-tuned UniXcoder model labels algorithm, purpose and quantum-vulnerable. XGBoost baseline. Served over gRPC. | `classifier/` | Python | 3 (dataset, extraction), 4 (training, serving) |
| Java config and certificate parser: nginx, Apache, SSH and Spring configs; X.509 and keystores through Bouncy Castle. | `server/` | Java 21 | 5 |
| Java orchestrator: runs scan jobs, merges duplicate findings, pushes live progress over WebSocket, scores risk (Mosca's inequality plus exposure and purpose), writes the CBOM. | `server/` | Java 21 | 5 |
| Java RAG planner: NIST documents parsed with Tika, chunked by section, embedded into pgvector, reranked; benchmark numbers looked up from the database; guards check every number and citation. Built with Spring AI. | `server/` | Java 21 | 6 |
| React dashboard: live progress, inventory table, risk heatmap, plan view with citations, benchmark charts, CBOM and PDF export. | `web/` | React, Vite | 6 |
| QuantumBank demo system and ground-truth file | `demo/quantumbank/` | Java, Python, C++, nginx | each member builds their language's part |

## Technology

| Layer | Technology |
|---|---|
| Server | Java 21, Spring Boot 3, Maven |
| RAG and LLM | Spring AI, Ollama (Qwen2.5) or a hosted LLM API |
| Database | PostgreSQL with pgvector |
| Document parsing | Apache Tika |
| Certificates | Bouncy Castle |
| CBOM | cyclonedx-core-java (CycloneDX 1.6) |
| Code parsing | tree-sitter (Python bindings) |
| ML | PyTorch, Hugging Face Transformers, UniXcoder, XGBoost; scikit-learn for evaluation |
| Binary analysis | C++17, LIEF, libyara |
| Benchmarks | C++17, liboqs and liboqs-cpp, OpenSSL 3.5 |
| Communication | gRPC, Protocol Buffers |
| Frontend | React, Vite, Recharts, Cytoscape.js |
| Deployment | Docker, Docker Compose |
| Demo extras | nginx with OpenSSL 3.5, Wireshark, Raspberry Pi 4 or 5 |
| RAG evaluation | RAGAS |

## Decisions

Recorded as ADRs in [decisions/](decisions/): [0001 gRPC instead of REST](decisions/0001-grpc-instead-of-rest.md), [0002 LLM and vector store](decisions/0002-llm-and-vector-store.md).
