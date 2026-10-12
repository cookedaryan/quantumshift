# 0001: gRPC instead of REST between services

## Status

Proposed (team to confirm).

## Context

The team playbook assumes two services (a Java server and a Python ML service) talking over a REST/JSON contract. QuantumShift has more moving parts: a C++ binary scanner, a Python classifier and a Java orchestrator, plus a C++ benchmark suite, in three languages (project plan Section 4.2). Every service must exchange the same `Finding` data, and the six members need to build in parallel without waiting for each other.

## Decision

Use gRPC with Protocol Buffers. The messages are defined once in `proto/findings.proto` (package `quantumshift.v1`), and C++, Java and Python code is generated from it. `docs/API.md` documents the contract. Any change to `proto/` needs an ADR.

## Alternatives rejected

- **REST/JSON with an OpenAPI file.** Matches the playbook and is easy to call by hand, but each language would hand-write or separately generate its own models, and the `Finding` shape could drift between three codebases.

## Consequences

- One source of truth for message shapes; generated code in all three languages.
- A build step (protobuf and gRPC code generation) in each language's build. This is Phase 1 work.
- The playbook's "REST contract" wording in `CLAUDE.md` and `CONTRIBUTING.md` is replaced by "gRPC contract in `proto/`".
- Debugging by hand needs a gRPC client (for example `grpcurl`) instead of `curl`.
