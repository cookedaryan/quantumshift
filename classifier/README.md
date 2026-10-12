# classifier

Python ML classifier. Extracts crypto candidates from Java, Python and C++ source with tree-sitter, then a fine-tuned UniXcoder model decides what is really crypto. Served over gRPC. See [CLAUDE.md](CLAUDE.md) for module rules.

Owners: role 3 (dataset collection, labelling guide, tree-sitter extraction; also builds the Python notification service) and role 4 (model training, evaluation, gRPC inference service; also writes the decoy code).

Status: not started.
