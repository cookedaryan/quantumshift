# QuantumShift

An AI-assisted planner for migrating software to post-quantum cryptography. QuantumShift finds old, quantum-breakable encryption (RSA and ECC) in an organisation's software, then writes a step-by-step plan to replace it with quantum-safe encryption. The plan follows NIST standards and is based on measured performance on the organisation's own kind of hardware.

B.Tech Computer Science major project, team of 6. Languages: C++, Python and Java, with an ML model and a RAG pipeline.

## What it does

1. Find every use of old encryption in source code, configuration files, certificates and compiled binaries.
2. Filter out false alarms with a machine-learning model.
3. Prioritise findings, so data that must stay secret for many years is fixed first.
4. Plan the replacement for each finding, citing the NIST standards and showing measured benchmark numbers.

## Repository layout

| Folder | What | Language |
|---|---|---|
| `scanner/` | Binary scanner (constants, imports, strings) | C++ |
| `bench/` | PQC benchmark suite and hybrid TLS demo setup | C++ |
| `classifier/` | Dataset, tree-sitter extraction, model training, gRPC inference | Python |
| `server/` | Orchestrator, config and certificate parser, risk scoring, CBOM, RAG planner | Java |
| `web/` | React dashboard | JavaScript |
| `demo/quantumbank/` | Fictional bank with planted old crypto, for the demo and accuracy tests | mixed |
| `proto/` | `findings.proto`, the shared gRPC contract | Protobuf |
| `docs/` | PRD, architecture, API, evaluation, ADRs, daily journal | Markdown |

## Run it

Nothing runs yet. The commands below are filled in during Phase 1.

```bash
cp .env.example .env              # TODO (Phase 1): fill in values
docker compose up                 # TODO (Phase 1): start all services
cd server && ./mvnw verify        # TODO (Phase 1): Java build and tests
cd classifier && pytest -q        # TODO (Phase 1): Python tests
cmake -S scanner -B build/scanner && cmake --build build/scanner && ctest --test-dir build/scanner   # TODO (Phase 1): C++ build and tests
```

## Where to read next

- [docs/PRD.md](docs/PRD.md): problem, objectives, scope
- [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md): components and data flow
- [docs/API.md](docs/API.md): the gRPC contract
- [docs/EVALUATION.md](docs/EVALUATION.md): metrics and baselines
- [CONTRIBUTING.md](CONTRIBUTING.md): how the team works, including with Claude
