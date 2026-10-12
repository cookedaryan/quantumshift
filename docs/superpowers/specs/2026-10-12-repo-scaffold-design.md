# QuantumShift repo scaffold: design

Date: 2026-10-12
Status: draft, awaiting review

## 1. Purpose

Set up the day-zero repository for QuantumShift (see `QuantumShift_Project_Plan.pdf`) following the
"Team AI Workflow Playbook". The goal is a repo where six team members and Claude Code can start
work immediately: shared context files, guardrails, contribution rules, CI skeleton, daily journal
automation, and docs drafted from the project plan.

Source documents:
- QuantumShift_Project_Plan.pdf (what we build, team roles, phases)
- Team AI Workflow Playbook, Major Project.pdf (how the team works with Claude and GitHub)

### Success criteria
- A teammate can clone the repo, read `CLAUDE.md` and `README.md`, and know the architecture,
  commands, conventions and their own module.
- Claude Code sessions start with correct, small context (root `CLAUDE.md` under ~150 lines,
  module rules only in module folders).
- CI is green on the empty skeleton and activates per language as code lands.
- Every number and claim in the docs traces to the project plan. Nothing is invented.

### Out of scope
- Feature code, build files (`pom.xml`, `CMakeLists.txt`, `pyproject.toml`), Docker Compose.
  These are Phase 1 work, separate from this setup.
- Creating the GitHub repo, secrets, branch protection, or the Projects board (needs the
  owner's account; listed as a manual checklist in section 7).
- Committing or pushing. Files are left uncommitted for review.

## 2. Directory layout

```
quantumshift/
├── CLAUDE.md                  root context, under ~150 lines
├── README.md                  what it is, how to run it (stub commands marked TODO until Phase 1)
├── CONTRIBUTING.md            branch, commit, PR, AI-usage rules
├── .env.example               every env var, no real values
├── .gitignore                 .env, data/raw, model weights, target/, venv/, build/, node_modules/
├── .claude/
│   ├── settings.json          shared permissions
│   ├── commands/              plan.md, review.md, handoff.md, adr.md
│   └── agents/code-reviewer.md
├── .github/
│   ├── CODEOWNERS             placeholder handles
│   ├── PULL_REQUEST_TEMPLATE.md
│   ├── ISSUE_TEMPLATE/        feature.md, bug.md
│   └── workflows/             ci.yml, daily-journal.yml
├── docs/
│   ├── PRD.md  ARCHITECTURE.md  API.md  EVALUATION.md
│   ├── decisions/             0001-grpc-instead-of-rest.md, 0002-llm-and-vector-store.md
│   ├── journal/               .gitkeep (filled by the 9 PM job)
│   └── superpowers/specs/     this file
├── proto/findings.proto       shared gRPC contract
├── scanner/                   C++ binary scanner       (README.md, CLAUDE.md)
├── bench/                     C++ PQC benchmarks       (README.md, CLAUDE.md)
├── classifier/                Python ML classifier     (README.md, CLAUDE.md)
├── server/                    Java orchestrator + RAG  (README.md, CLAUDE.md)
├── web/                       React dashboard          (README.md)
├── demo/quantumbank/          fictional bank + ground truth (README.md)
└── scripts/write_journal.py
```

The playbook's `server/` + `ml/` split becomes one folder per service, matching plan Section 4.
Module ownership follows plan Section 10:

| Folder | Owner (plan role #) |
|---|---|
| `scanner/` | 1: C++ binary scanner |
| `bench/` | 2: C++ benchmarks, Pi, hybrid TLS demo |
| `classifier/` | 3: dataset, tree-sitter; 4: model training, gRPC inference |
| `server/` | 5: orchestrator, config parser, risk, CBOM; 6: RAG pipeline, guards |
| `web/` | 6: React dashboard |
| `demo/quantumbank/` | each member adds their own language's part |
| `proto/`, `docs/ARCHITECTURE.md` | tech lead (role to be assigned) |

## 3. Deviations from the playbook

1. **gRPC, not REST.** The plan specifies gRPC and Protocol Buffers between C++, Java and Python.
   `docs/API.md` documents `findings.proto` and the service calls. ADR 0001 records the deviation.
   The playbook rule "never change the contract without an ADR" applies to `proto/`.
2. **Per-language CI.** `ci.yml` has three jobs: C++ (CMake + ctest), Python (ruff + pytest),
   Java (`./mvnw verify`). Each job runs only when its folder contains its build file, so CI stays
   green on the empty skeleton.
3. **Permissions.** `.claude/settings.json` allows `cmake`, `ctest`, `pytest`, `./mvnw` and
   read-only git. It denies reading `.env*`, `git push`, and `rm -rf`. The rule syntax must be
   checked against current Claude Code settings docs when written (as the playbook says).
4. **Journal model.** `scripts/write_journal.py` reads the model ID from the `JOURNAL_MODEL`
   environment variable, default `claude-haiku-4-5-20251001`. The playbook's `claude-haiku-5-5`
   could not be verified. A weekly Sunday summary job (playbook suggestion) is included.
5. **Language-specific conventions** replace the playbook's Java/Python-only ones: C++17 with
   CMake and clang-format, Python with type hints and ruff, Java 21 with Spring Boot and
   constructor injection.

## 4. Contents of key files

- **Root `CLAUDE.md`:** what QuantumShift is (plan Section 1), architecture and gRPC contract
  pointer, commands per module, conventions, "Rules for Claude" from the playbook (plan first,
  stay in the folder, tests before done, never read `.env`, never commit/push, never invent
  metrics), definition of done. Imports `@docs/ARCHITECTURE.md`.
- **Module `CLAUDE.md` files:** short, language-specific rules. `scanner/` notes the endianness
  and stripped-binary requirements; `classifier/` the 70/15/15 split and test-set discipline;
  `server/` the guard rule (numbers only from the benchmark database).
- **`docs/PRD.md`:** problem, objectives O1-O7, scope tables, and success metrics from plan
  Sections 3 and 9.
- **`docs/ARCHITECTURE.md`:** components, data flow and tech stack from plan Sections 4, 5, 7.
- **`docs/API.md`:** `findings.proto` Finding message and the service calls between orchestrator,
  scanner, classifier and benchmark data.
- **`docs/EVALUATION.md`:** baselines (Semgrep, XGBoost, findcrypt-yara, sonar-cryptography),
  metrics (precision, recall, F1, citation accuracy, faithfulness, numeric accuracy), with empty
  result tables. No numbers filled in. Includes a licence-check section for datasets and models.
- **`proto/findings.proto`:** the Finding message from plan Section 4.2 verbatim.
- **ADRs:** 0001 gRPC instead of REST. 0002 LLM and vector store (Ollama/Qwen2.5 or hosted API,
  PostgreSQL + pgvector), recording the alternatives the plan names.
- **GitHub files:** PR template with AI-usage section, feature and bug issue templates,
  CODEOWNERS mapping the table in section 2.

## 5. Placeholders

- CODEOWNERS uses handles such as `@member1-cpp-scanner`; replace with real GitHub usernames.
- The tech lead is not assigned in the plan. Marked `TODO(assign)` in `CLAUDE.md`, CODEOWNERS and
  `CONTRIBUTING.md`.
- README run commands are marked TODO until Phase 1 produces a working `docker compose up`.

## 6. Verification

- `proto/findings.proto` compiles with `protoc` if available; otherwise checked by reading.
- YAML files (`ci.yml`, `daily-journal.yml`, issue templates) parse with a YAML loader.
- `settings.json` parses as JSON. `write_journal.py` passes `python -m py_compile`.
- Root `CLAUDE.md` line count is checked against the ~150 limit.
- Grep confirms no real secrets or API keys are present.

## 7. Manual steps for the team (not done by Claude)

1. Create the GitHub repo and push.
2. Replace CODEOWNERS placeholders; assign the tech lead.
3. Protect `main`: PR required, green CI, one CODEOWNER approval, squash-merge.
4. Create the GitHub Projects board (Backlog, This week, In progress, In review, Done; limit of
   two in progress per person).
5. Create and pin the "Daily log" issue; set `LOG_ISSUE` in `daily-journal.yml`.
6. Add the `ANTHROPIC_API_KEY` repo secret, set a spend limit in the Claude Console, create the
   `journal` label, enable "Allow GitHub Actions to create and approve pull requests".
7. Run the journal workflow once by hand (`workflow_dispatch`) before relying on the schedule.
