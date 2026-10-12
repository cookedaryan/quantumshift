# QuantumShift

## What this is
An AI-assisted planner for migrating software to post-quantum cryptography. It finds RSA/ECC in source code, configs, certificates and binaries, filters false alarms with an ML classifier, ranks findings by quantum risk, and writes NIST-cited migration plans with a RAG planner using measured benchmark numbers. B.Tech major project, team of 6. Tech lead: TODO(assign).

## Architecture
- scanner/ - C++17. Binary scanner (LIEF, YARA rules). Role 1.
- bench/ - C++17. PQC benchmarks (liboqs, OpenSSL 3.5), hybrid TLS demo. Role 2.
- classifier/ - Python 3.12. Dataset, tree-sitter extraction, UniXcoder model, gRPC inference. Roles 3, 4.
- server/ - Java 21, Spring Boot 3, PostgreSQL + pgvector. Orchestrator, config parser, risk scoring, CBOM, RAG planner. Roles 5, 6.
- web/ - React + Vite dashboard. Role 6.
- demo/quantumbank/ - fictional bank with planted old crypto and a ground-truth file.
- Services talk over gRPC. The contract is proto/findings.proto, documented in docs/API.md. Never change it without an ADR.

@docs/ARCHITECTURE.md

## Commands
- C++ build + tests (per module, scanner or bench): `cmake -S scanner -B build/scanner && cmake --build build/scanner && ctest --test-dir build/scanner`
- Python tests: `cd classifier && pytest -q`
- Java build + tests: `cd server && ./mvnw verify`
- Run everything: `docker compose up` (TODO Phase 1: not set up yet)
- Nothing is built yet; commands become real as each module gets its build file.

## Conventions
- Branch: feature/<issue>-<slug>. Commits: Conventional Commits (feat:, fix:, docs:).
- C++: C++17, CMake, clang-format.
- Python: type hints, ruff format, no notebooks inside classifier/src.
- Java: constructor injection, DTOs at the API edge, no business logic in controllers.
- Every module has its own CLAUDE.md with extra rules. They load only when you work in that folder.

## Rules for Claude
- Plan first; wait for approval before editing more than one file.
- Stay inside the folder the task is about; ask before touching another module.
- Add or update tests with every change and run them before saying "done".
- Never read or print .env files, keys or tokens. Never commit, push or merge.
- If unsure, ask. Never invent APIs, results or metrics.
- Numbers in plans or reports come only from the benchmark database or docs/EVALUATION.md, never from model text.
- Changes to proto/, CLAUDE.md or .claude/ happen only in their own PR.

## Definition of done
Tests pass, docs/API.md updated if the contract changed, ADR written if the change crosses modules, PR template filled in.
