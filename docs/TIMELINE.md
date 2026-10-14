# Timeline (draft)

A week-by-week schedule for the issues in [BACKLOG.md](BACKLOG.md). **Nothing here is decided until the team reviews it.**

## Assumptions

- The project plan gives no end date, so this assumes **20 weeks, Mon 12 Oct 2026 to Sun 28 Feb 2027**, with the code freeze at the end of week 20. **There is no spare buffer.** If your real deadline is later, the extra weeks are your buffer. If it is earlier, cut scope (see the cut order in the backlog notes) before squeezing the thin-path milestone.
- Estimated effort per person is 19 to 45 focused days (R1 19, R2 22, R3 22, R4 23, R5 35, R6 45; see the backlog notes). If each of you has about 2 to 3 focused days a week, that is 40 to 60 days over 20 weeks. R6 and R5 are the risk; rebalance before starting.
- Weeks 11 and 12 (21 Dec to 3 Jan) are planned light for exams and holidays. Adjust to your academic calendar.
- Sizes are focused work days and do not include reviews, bug fixes, meetings or classes.

## Milestones and gates

| Milestone | Weeks | Ends | Exit check (gate) |
|---|---|---|---|
| M1 Foundations | 1-2 | Sun 25 Oct 2026 | **Gate A:** `docker compose up` runs every skeleton; CI green on all three languages |
| M2 Thin end-to-end path | 3-7 | Sun 29 Nov 2026 | **Gate B:** recorded demo of QuantumBank scan, inventory, one risk priority, CBOM and one plan (M2-22). If missed, do not start new M3 work until the pipeline connects |
| M3 Intelligence | 8-13 | Sun 10 Jan 2027 | **Gate C:** model and scanner compared with their baselines on validation data; guards working. Decide which stretch items to drop |
| M4a Core measurement | 14-16 | Sun 31 Jan 2027 | **Gate D:** feature-complete on the `laptop` profile |
| M4b Hardware-aware block | 17-18 | Sun 14 Feb 2027 | **Gate E:** Pi profile and hardware-aware plans done and planner frozen, **or** fall back to laptop-only |
| M5 Evaluation and polish | 19-20 | Sun 28 Feb 2027 | **Gate F:** code freeze, final numbers in `docs/EVALUATION.md`, rehearsed demo and backup video |

## Week by week

| Week | Dates | Focus | Issues |
|---|---|---|---|
| 1 | 12-18 Oct | GitHub setup, contract, labelling guideline, datasets, Pi smoke test | M1-01, M1-02, M1-08, M1-09, M1-10, M1-11, M1-14 |
| 2 | 19-25 Oct | Skeletons for all services, compose, first labels | M1-03 to M1-07, M1-12, M1-13 |
| 3 | 26 Oct-1 Nov | Extraction, scanner imports and strings, parsers, benchmarks start, RAG ingestion start, banking API | M2-01, M2-02, M2-04, M2-05, M2-06, M2-10, M2-12, M2-15 |
| 4 | 2-8 Nov | Rule-based classifier, size check, keystore, notification service, nginx config | M2-03, M2-11, M2-16, M2-17, M2-19 |
| 5 | 9-15 Nov | Orchestrator, basic RAG plan, ATM firmware, decoys | M2-07, M2-13, M2-18, M2-20 |
| 6 | 16-22 Nov | Risk score, CBOM, minimal dashboard, ground truth | M2-08, M2-09, M2-14, M2-21 |
| 7 | 23-29 Nov | Thin-path demo, fix what broke | M2-22 (Gate B) |
| 8 | 30 Nov-6 Dec | Finish labelling and freeze splits, scanner constants, section-aware chunking | M3-01, M3-08, M3-12 |
| 9 | 7-13 Dec | Baselines, fine-tuning, endianness, reranker, benchmark lookup | M3-03, M3-04, M3-05, M3-09, M3-13, M3-14 |
| 10 | 14-20 Dec | Stage-1 recall, evaluation script, stripped firmware, structured plans | M3-02, M3-06, M3-10, M3-15 |
| 11 | 21-27 Dec | **Light week.** Full risk factors, start live progress, catch up | M3-17, M3-18 (start) |
| 12 | 28 Dec-3 Jan | **Light week.** Model in service, binary baseline, guards | M3-07, M3-11, M3-16 |
| 13 | 4-10 Jan | Live progress, dashboard filters and heatmap, catch up | M3-18, M3-19 (Gate C) |
| 14 | 11-17 Jan | Laptop benchmark matrix, peak memory, laptop handshake numbers, ML-KEM migration start | M4-01, M4-02, M4-04, M4-07 |
| 15 | 18-24 Jan | Hybrid TLS demo, citations, CBOM download, source and binary evaluations, report drafts; **book the faculty reviewer** | M4-05, M4-07, M4-09, M4-10, M5-01, M5-02 |
| 16 | 25-31 Jan | Full-pipeline run on the laptop profile, licence audit, real-repo spot check start | M4-11, M5-10, M5-15 (Gate D) |
| 17 | 1-7 Feb | Pi benchmarks and handshake numbers, whole-tool comparison, real-repo spot check | M4-03, M4-12, M5-03, M5-15 |
| 18 | 8-14 Feb | Hardware-aware plans, profile-switch charts, **planner freeze** | M4-06, M4-08, M4-13 (Gate E) |
| 19 | 15-21 Feb | RAG evaluation (after the freeze), accuracy panel, performance numbers, re-check standards, report sections, slides and demo script | M5-04, M5-06, M5-08, M5-09, M5-11, M5-12 |
| 20 | 22-28 Feb | Expert review ratings, backup video, three timed rehearsals, viva prep, PDF export only if time remains | M5-05, M5-13, M5-14, M5-07 (Gate F) |

## Keeping the hardware-aware part at the end

The plan's hardware-aware idea (benchmarks on a laptop and a Raspberry Pi, with plans that change by hardware) is scheduled in M4b. That works if you keep four rules:

1. **Pi smoke test in week 1 (M1-09).** Building liboqs and OpenSSL 3.5 on ARM is the plan's main hardware risk. Find out in week 1 whether it builds, not in week 17. Full Pi benchmarking waits.
2. **Hardware profile as data from M2.** The benchmark table has a `hardware_profile` column (M2-10) and the planner takes a profile parameter, default `laptop` (M3-14). Only `laptop` is populated until M4b, so week 17 is adding data and rules, not a redesign.
3. **Planner freeze before the RAG evaluation (M4-13 before M5-04).** Hardware rules change plan outputs. Run RAGAS, citation accuracy and numeric accuracy only after the freeze, or you will have to repeat them.
4. **Fallback.** If the Pi slips, drop M4-03, M4-12 and M4-06, keep the freeze, and say clearly in the report that the hardware-aware part is partial. The saved Pi chart for the demo exists only if the Pi results exist.

Risk of this ordering: your main differentiator is built last, under time pressure. To reduce it, start the profile-switching logic in week 14 with laptop numbers and placeholder rules, and swap in Pi data in week 17.

## Things that can break this schedule

| Risk | Where it bites | Mitigation |
|---|---|---|
| Labelling is slow | M3-01 (week 8) | Started in week 1; rules pre-label and people correct; public benchmarks first |
| R6 and R5 overloaded | Weeks 3-13 | Rebalance using the backlog notes before starting |
| Three languages fail to integrate | Gate B (week 7) | One shared `.proto` from week 1; thin path before intelligence |
| Pi build fails | Week 1 smoke test | Docker-image fallback; borrow a Pi; label any substitute honestly in `docs/EVALUATION.md` |
| Exams in weeks 11-12 | M3 | Those weeks are planned light; move M3-17 and M3-18 if needed |
| Faculty reviewer unavailable | M5-05 (week 20) | Book in week 15 |
| Standards change | M5-09 | Re-check NIST pages in week 19 and re-index |
