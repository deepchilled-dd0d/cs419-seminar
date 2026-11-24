# CS419 seminar project overview

**Version**: 1.0.0
**Last updated:** 24/11/2026  
**Working topic:** ColPali: Efficient Document Retrieval with Vision Language Models  
**Deadline window:** 10/12/2026–17/12/2026; exact presentation and submission dates remain unconfirmed.

## Purpose

Our group must read a research paper, understand and summarize its contributions, present it, test-run the method, and report our findings. The seminar should explain both what the authors demonstrated and what our own experiments establish, including limitations and possible improvements.

## Sources and course requirements

- [Course slide](../assets/image.png): the group assignment is worth 4.0 points, split into paper reading/seminar (2.0) and a separate project component (2.0, marked TBU). Paper reading includes reading and preparing presentation slides, running experiments and identifying limitations, and improvement or discussion. AI tools are allowed. Groups have 4–5 students.
- [Supplied paper](../assets/ColPali.pdf): ICLR 2025 conference version, arXiv:2407.01449v6, dated 28/02/2025. This is the working paper for the seminar.

## Team

| Member | Role |
| --- | --- |
| Lạc | Read and summarize paper, make presentation. Present the introduction, problem, and paper contribution. |
| Kỳ | Implement baseline to run on Kaggle. Write and present the background section.|
| Duyên | Write the method section, present the ColPali architecture and late interaction explanation. |
| Thịnh | Write the dataset and results sections, present evaluation protocol, metrics, and results analysis. |
| Nhi | Write the limitations and conclusion sections and present them. Review report and presentation slides. |

## Paper and guiding question

**Guiding question:** How does retrieving directly from document-page images with multi-vector embeddings and late interaction compare with a text-based retrieval pipeline, and what quality and resource trade-offs can we observe in a small test run?

The paper addresses retrieval from visually rich documents, where tables, charts, layout, and figures may be lost in text extraction. It contributes the Visual Document Retrieval Benchmark (ViDoRe) and ColPali, which embeds page images rather than requiring an OCR/layout/chunking pipeline for its document representation.

Key concepts to cover:

- Page-level retrieval: rank pages for a text query; answer generation is a separate downstream task.
- PaliGemma-3B backbone with a projection to 128-dimensional token/patch vectors (paper §4.1).
- Multi-vector query and page representations. Late interaction sums, over query vectors, the maximum dot product with any page vector: `score(q, d) = sum_i max_j dot(q_i, d_j)`.
- Offline page encoding/indexing versus online query encoding and scoring.
- Training with a contrastive objective and LoRA adapters (paper §4.2); LoRA is supporting background, not the seminar's central topic.
- ViDoRe, nDCG@5, comparison baselines, ablations, and efficiency measurements (paper §§3–6 and appendices). Attribute published measurements to the paper and preserve their evaluation conditions.

## Planned experiment scope

The minimum target is a reproducible inference/evaluation run using a released ColPali checkpoint and a small, fixed evaluation subset, plus a text-based BM25 baseline on the same pages and queries. Full training, reproducing every ViDoRe result, and building a complete RAG system are outside the default scope.

Before the full run, record available GPU/VRAM, runtime access, storage, model access conditions, and dependency compatibility. Pin the checkpoint revision, dataset revision, packages, and commands that actually work. Choose an evaluation subset with relevance labels, ideally from a ViDoRe test split; record exact page/query IDs and sampling seed. A working target is 50–100 pages and 20–30 labeled queries, adjusted after the hardware smoke test. This size is a team planning target, not a course requirement.

Evaluate both systems with identical candidate pages, queries, relevance labels, and ranking conventions. Preserve the full candidate pool for the chosen subset, identify the text extraction method used by BM25, and discuss how extraction failures affect the comparison. Report nDCG@5 and Recall@5, with documented relevance handling. Record indexing time, query timing with its measurement boundary, index size, and peak GPU memory when measurable. Keep hardware, batch size, dtype, warmup, and timing repetitions alongside measurements.

Inspect successful and failed retrievals across text, table/chart, and layout-heavy cases where the data supports them. Save rankings and qualitative examples. A smaller subset is acceptable when resources constrain the run, provided its limits are explicit. A demo without labeled evaluation must be described as a qualitative test rather than a benchmark reproduction. If model execution is blocked, document errors, try a smaller batch/subset or available hosted GPU, and escalate early; published results alone do not complete the test-run objective.

An improvement experiment is optional after the core comparison works. Discussion of limitations and plausible improvements is required even if no modification is implemented.

## Deliverables and completion criteria

1. Reading notes in `docs/summary/` covering motivation, related work, method, evaluation, contributions, and limitations, with paper section/page references.
2. Runnable experiment code or notebook in `src/`, environment instructions, fixed evaluation manifest, raw rankings, metrics, and execution logs sufficient for another member to rerun.
3. A report in `src/report/` containing a paper summary, our experimental setup, results, error analysis, limitations, discussion, conclusion, and references. Clearly separate **paper findings**, **team measurements**, and **interpretation/proposals**.
4. Presentation slides in `src/presentation/` that closely follows content report and execution logs.
5. A rehearsal and shared Q&A preparation (in `docs/summary/`) involving all five members.

Use `src/` for code/notebooks, `docs/summary/` for reading notes, `docs/results/` for compact results and logs, `docs/report/` for report sources, `docs/presentation/` for slide sources and the demo backup, and `docs/assets/` for supporting figures. Avoid committing model weights, full datasets, caches, or generated LaTeX PDFs. Cite borrowed figures, tables, equations, and findings.

## Schedule and coordination

Plan from 24/11/2026, finish research and experiments by 02/12, draft the report and slides by 06/12, and complete review and rehearsal by **09/12**. This internal readiness date protects against the earliest date in the 10–17 December window. Use the remaining window for the confirmed presentation/submission and necessary corrections.

Each task has an owner, deadline, dependency, and reviewable deliverable. Work on branches named with task IDs, open a PR when ready, and communicate blockers before they threaten dependent tasks. Follow `snake_case` for new filenames and code identifiers while preserving official titles and existing supplied filenames. Do not copy the obsolete LoRA assignments into the new plan.

Track executable work in [tasks.md](tasks.md). Until the instructor specifies otherwise, a 15-minute talk plus approximately 5 minutes of Q&A is only a provisional planning assumption inherited from `ref.md`, with speaking time shared across all five members.
