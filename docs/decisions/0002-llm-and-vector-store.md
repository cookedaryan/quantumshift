# 0002: LLM and vector store for the RAG planner

## Status

Proposed (team to confirm). The project plan states these choices; the team has not yet decided them.

## Context

The RAG planner (project plan Section 5.8) retrieves passages from NIST and IETF documents and benchmark numbers, and asks an LLM to write a cited migration plan. It needs a place to store findings, benchmark results and text embeddings, and an LLM that the Java server can call. The team is on a student budget.

## Decision

- **Vector store:** PostgreSQL with the pgvector extension. One database holds findings, benchmarks and embeddings.
- **LLM:** a local open model (Qwen2.5 through Ollama) or a hosted LLM API, both reached through Spring AI so the choice is a configuration change.

## Alternatives rejected

- **A separate vector database** (for example Chroma or Qdrant). More capable search features, but a second system to run, secure and back up, and benchmark numbers would still need a relational store.
- **Hosted LLM only.** Better answer quality is likely, but it costs money per run and sends document text to a third party. A local model is free and works offline for the demo, at some quality cost.

## Consequences

- One database to run in Docker Compose.
- Retrieval quality depends on chunking by document section and on the reranker, not on the vector store.
- The guards (every number from the benchmark table, every citation a retrieved passage) matter more with a smaller local model, which hallucinates more often.
- Re-check this ADR if the local model's plan quality, measured by RAGAS faithfulness, is too low.
