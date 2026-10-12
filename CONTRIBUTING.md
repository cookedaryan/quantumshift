# Contributing to QuantumShift

The repo is the single source of truth. Work happens through issues and pull requests. Claude drafts, plans and reviews; people own decisions, approvals and merges.

**Tech lead / integrator:** TODO(assign). Merges to `main`, owns `CLAUDE.md`, `docs/ARCHITECTURE.md`, `docs/decisions/`, the weekly sync, the API key and the journal job.

## Rules

1. All work starts as a GitHub issue with acceptance criteria. No issue, no branch.
2. One issue, one branch (`feature/42-rag-chunking`), one PR. Keep PRs under about 400 changed lines.
3. `main` is protected: a PR, green CI and one approval from the folder's CODEOWNER are required. Squash-merge.
4. Fill in the PR template, including the AI-usage section.
5. Anything crossing modules needs an ADR merged first: a change to `proto/findings.proto`, a new dependency, or a database schema change.
6. Changes to `CLAUDE.md` or `.claude/` go through a PR the tech lead approves. One edit changes how Claude behaves for all six people.
7. Post your daily update on the pinned **Daily log** issue by 8:30 PM IST: Done / Next / Blocked. The 9 PM journal job reads it.
8. Review requests get an answer within 24 hours. Older ones are flagged in the journal.

## Conventions

- Branches: `feature/<issue>-<slug>`.
- Commits: Conventional Commits (`feat:`, `fix:`, `docs:`).
- Issues: use the feature or bug template. Nothing larger than 3 days of work.
- Board: Backlog, This week, In progress, In review, Done. At most two items in progress per person.
- ADRs: a half-page file in `docs/decisions/` (status, context, decision, alternatives rejected, consequences). Use `/adr <title>` in Claude Code.

## Working with Claude

- Run `claude` from the repo root. One session per issue; run `/clear` before unrelated work.
- Start with `/plan-issue <issue>`. Use plan mode (Shift+Tab) for anything that touches more than one file.
- Commit small and often, so a bad AI change is one `git revert` away.
- Run `/self-review` before opening a PR. (The names avoid Claude Code's built-in `/plan` and `/review`, which a project command cannot reliably replace.) Run `/handoff <issue>` when passing work on.

## Ground rules for AI use

- The team owns every decision and every merged line. Anyone who merges code must be able to explain it.
- Architecture, datasets and evaluation metrics are decided by people and recorded as ADRs. Claude drafts; the team decides.
- Numbers in the report come from scripts you ran and logged in `docs/EVALUATION.md`, never from model text.
- No secrets in prompts, code or commits. If a key leaks, rotate it first, then clean the history.
- Check dataset and model licences before use and note them in `docs/EVALUATION.md`.
- Shared files (`CLAUDE.md`, `docs/API.md`, `proto/`) change only in their own PR, never as a side effect.
- Read your college's policy on AI tools and disclose use in the report. The PR AI-usage sections are your record.
