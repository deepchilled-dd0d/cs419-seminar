# Task division: ColPali seminar

**Version:** 1.2.0  
**Last edited:** 24/11/2026  
**Project overview:** [overview.md](overview.md)  
**Task-plan reference:** [ref.md](ref.md)  

## Rules

- Every task has a deadline and a reviewable deliverable. Completion means the artifact is available on GitHub or, for large/external artifacts, its location and access instructions are shared with the team. Presentation tasks complete when delivered.
- Each row has a primary owner; named reviewers/supporting members help without removing the owner's responsibility. All tasks start as **not started**; mark completed task IDs in PRs or update the table when progress changes.
- Communicate blockers promptly, including their effect on dependent tasks. Lạc coordinates scope changes and confirms instructor requirements.
- Use `snake_case` for new filenames, directories, and code identifiers. Preserve official paper titles, proper names, and supplied asset filenames.
- For one or more related tasks, create a branch named `[Task 1 ID]_[Task 2 ID]_...` and open a PR for review. Each member reviews at least one other member's work. For simplicity's sake, we only have the `main` branch and feature branches; approved features are merged to `main` directly.
- Do not commit generated LaTeX PDFs, model weights, full datasets, or caches. Commit sources, environment instructions, compact results, and artifact links. 
- Separate paper findings, measured team results, and proposed improvements. Cite the paper and any external code, figures, or data used.
- Dates below use `DD/MM/YYYY` and local time (Asia/Ho_Chi_Minh), with end-of-day deadlines unless a meeting time is agreed. Exact course deadlines override this plan once confirmed.

Use `docs/summary/summary.tex` for the shared paper summary, `docs/report/` for report sources, `docs/presentation/` for presentation sources and demo backups, `src/` for experiment code/notebooks, and `docs/results/` for saved outputs. These paths follow the LaTeX files now present in the repository.

## Phases

Follow the AI seminar reference's research → writing → presentation structure. Research starts with Lạc finishing `summary.tex`; the other research tasks build on that shared summary. Writing tasks are listed in report order, followed by report compilation, slide preparation, and review. Deadlines and dependencies determine when work begins; independent tasks may proceed together after their prerequisites are ready.

### Research

Expected window: 24/11/2026–02/12/2026.

| Task ID | Task description | Assignee | Deliverable | Deadline | Dependencies / review |
| --- | --- | --- | --- | --- | --- |
| RE-01 | Finish the paper summary: motivation, problem, contributions, architecture, training, evaluation, published findings, and limitations, with paper section/page references. | Lạc | `docs/summary/summary.tex` | 25/11/2026 | Starting task. Share the completed summary with all members so it provides the foundation for RE-02–RE-04. |
| RE-02 | Read the summary and explain page-level retrieval, text extraction/OCR, BM25, dense retrieval, and why visual information matters. | Kỳ | `docs/summary/retrieval_background.tex` | 26/11/2026 | Depends on RE-01; Duyên reviews. Supplies the baseline rationale and report background. |
| RE-03 | Explain ColPali's architecture, query/page embeddings, MaxSim equation and dimensions, contrastive training, and LoRA's supporting role. Draw the indexing/query pipeline and work through a small scoring example. | Duyên | `docs/summary/colpali_method.tex`; `docs/assets/colpali_pipeline.svg` | 27/11/2026 | Depends on RE-01; Lạc reviews. Supplies the method section and technical Q&A. |
| RE-04 | Define the evaluation subset, candidate pages, queries, relevance labels, seed, nDCG@5/Recall@5 calculation, and timing protocol. Record exact dataset revision and page/query IDs. | Thịnh | `docs/summary/evaluation_protocol.tex`; `docs/results/evaluation_manifest.json` | 28/11/2026 | Depends on RE-01; Kỳ reviews feasibility. Keep relevant pages in the fixed candidate pool; document sampling bias and freeze the protocol before RE-05. |
| RE-05 | Implement and test-run BM25 and a released ColPali checkpoint on Kaggle using the same fixed pages and queries. Start with a small smoke test, then save rankings, extraction failures, run logs, and resource measurements. | Kỳ | `src/bm25_evaluation.ipynb`; `src/colpali_evaluation.ipynb`; `src/requirements.txt`; `src/README.md`; rankings and run metadata in `docs/results/` | 30/11/2026 | Depends on RE-02–RE-04. Pin model/data/package revisions and record Kaggle notebook versions, GPU/VRAM, dtype, batch size, indexing/query timing boundaries, and index size. Duyên checks the implementation; Lạc supports blockers. |
| RE-06 | Evaluate both systems with one metric implementation; check a hand-calculated ranking and compare retrieval quality, indexing/query time, index size, and memory where measurable. | Thịnh | `src/evaluate_retrieval.py`; `docs/results/metrics.json`; `docs/results/comparison.md` | 01/12/2026 | Depends on RE-05; Kỳ reviews IDs and metric conventions. Save raw results and distinguish team measurements from published findings. |
| RE-07 | Analyze successful and failed retrievals, evaluation caveats, resource trade-offs, and limitations. Propose improvements grounded in the paper and team results. | Nhi | `docs/summary/limitations_and_improvements.tex`; `docs/results/error_analysis.md`; example images in `docs/assets/` | 02/12/2026 | Depends on RE-01/RE-06; Thịnh supports interpretation. Label untested proposals and discuss differences from the paper's experimental setup. |
| RE-08 | Independently rerun a small slice using the saved Kaggle instructions and verify page IDs, rankings, and metric outputs. Record discrepancies and fixes. | Duyên | `docs/results/reproduction_check.md`; corrections to `src/README.md` if needed | 02/12/2026 | Depends on RE-05/RE-06; Kỳ supports fixes. Confirms the experiment is ready to use in the report. |

If the Kaggle smoke test in RE-05 fails, Kỳ records errors and tries smaller batches or a feasible runtime; Lạc coordinates scope changes with Thịnh. Update the shared manifest and both systems' runs if the subset changes. A qualitative demo must be labeled as such; published results alone do not complete the test-run objective. An improvement experiment is optional after the core comparison works.

### Writing

Expected window: 03/12/2026–08/12/2026. Use LaTeX section sources under `docs/report/`, assembled by `main.tex`. As in the reference, the abstract appears first in the task list but is written after the main sections have drafts.

| Task ID | Task description | Assignee | Deliverable | Deadline | Dependencies / review |
| --- | --- | --- | --- | --- | --- |
| WR-01 | Write the abstract summarizing the paper, our test run, and the actual findings. | Lạc | `docs/report/abstract.tex` | 06/12/2026 | Write after WR-02–WR-08 have drafts; Nhi reviews. |
| WR-02 | Write the introduction: motivation, problem, guiding question, paper contributions, and seminar scope. | Lạc | `docs/report/introduction.tex` | 03/12/2026 | Draw on RE-01; Nhi reviews. |
| WR-03 | Write the background and related work: text extraction, BM25, dense retrieval, and visual retrieval. | Kỳ | `docs/report/background.tex` | 03/12/2026 | Draw on RE-02; Duyên checks accuracy and Nhi reviews clarity. |
| WR-04 | Write ColPali's method, pipeline, and scoring example. Separate the authors' training procedure from our inference experiment. | Duyên | `docs/report/method.tex` | 04/12/2026 | Draw on RE-01/RE-03; Lạc reviews. |
| WR-05 | Write the dataset and experimental setup: subset, candidates, labels, model, baseline, hardware, metrics, and reproducible procedure. | Thịnh | `docs/report/dataset.tex`; `docs/report/experimental_setup.tex` | 04/12/2026 | Draw on RE-04–RE-06/RE-08; Kỳ verifies Kaggle details. |
| WR-06 | Write results and discussion with comparable tables, explicit measurement conditions, and retrieval examples. | Thịnh | `docs/report/results.tex` | 05/12/2026 | Draw on RE-06/RE-07 and WR-05; Kỳ reviews. Separate paper findings, team measurements, and interpretation. |
| WR-07 | Write limitations, failure analysis, and proposed improvements. | Nhi | `docs/report/limitations.tex` | 05/12/2026 | Draw on RE-07 and WR-04–WR-06 drafts; Duyên reviews. State what the small experiment can establish. |
| WR-08 | Write the conclusion, answering the guiding question using the evidence presented. | Nhi | `docs/report/conclusion.tex` | 06/12/2026 | Depends on WR-02–WR-07 drafts. Distinguish tested findings from proposed improvements. |
| WR-09 | Compile and format references, integrate all sections, and compile the report. Resolve citations, asset paths, terminology, and unsupported claims. | Lạc | `docs/report/main.tex`; `docs/report/references.bib`; build instructions in `docs/report/README.md` | 07/12/2026 | Depends on WR-01–WR-08; Nhi checks the compiled report. Compile PDFs locally; commit sources and instructions. |
| WR-10 | Make the presentation from the research assets, report drafts, and execution logs. Add citations on relevant slides. | Lạc | `docs/presentation/presentation.tex`; build instructions and saved demo outputs in `docs/presentation/` | 06/12/2026 | Draw on RE-01–RE-08 and WR-02–WR-08 drafts. Each speaker checks their section; keep slides synchronized with the final report. Kỳ supplies the demo backup from RE-05 outputs. |
| WR-11 | Review the report and presentation for accuracy, consistency with saved results, formatting, citations, coverage, and timing. Verify corrections. | Nhi (all members review their sections) | `docs/management/review_checklist.md` with feedback and resolved issues | 08/12/2026 | Depends on WR-09/WR-10; Thịnh verifies results, Lạc coordinates fixes. |

### Presentation

Preparation and rehearsal: 08/12/2026–09/12/2026. Delivery: the course presentation date within 10/12/2026–17/12/2026. All artifacts should be ready by **09/12/2026**.

Plan provisionally for 10 content slides in 15 minutes, followed by approximately 5 minutes of Q&A (based on AI seminar, idk the exact time for this one yet). Title and references slides are separate. Adjust to the course's final allotted duration. Each member has 3 minutes of planned speaking time.

| Task ID | Task description | Assignee | Deliverable | Deadline | Dependencies / note |
| --- | --- | --- | --- | --- | --- |
| PR-01 | Introduce the problem, guiding question, and paper contributions. | Lạc | Slides 1–2; notes in `docs/summary/speaker_notes.md` | Notes: 08/12/2026; delivery: course presentation date | RE-01/WR-02/WR-10; 3 minutes. |
| PR-02 | Present retrieval background and the text-based baseline implemented on Kaggle. | Kỳ | Slide 3; speaker notes | Notes: 08/12/2026; delivery: course presentation date | RE-05/WR-03/WR-10; 3 minutes. |
| PR-03 | Explain ColPali architecture, offline/online encoding, and late interaction scoring. | Duyên | Slides 4–5; speaker notes | Notes: 08/12/2026; delivery: course presentation date | WR-04/WR-10; 3 minutes. |
| PR-04 | Present the dataset, evaluation protocol, metrics, published-versus-team results, and a saved retrieval example. | Thịnh | Slides 6–8; speaker notes | Notes: 08/12/2026; delivery: course presentation date | WR-05/WR-06/WR-10; 3 minutes including the example. Use the offline backup if a live demo would overrun. |
| PR-05 | Present failure cases, limitations, possible improvements, and conclusion. | Nhi | Slides 9–10; speaker notes | Notes: 08/12/2026; delivery: course presentation date | WR-07/WR-08/WR-10; 3 minutes. |
| PR-06 | Rehearse the full presentation, transitions, and demo fallback; fix overruns and unclear explanations. | Lạc (all members participate) | `docs/summary/rehearsal_notes.md` with timings and resolved actions | 09/12/2026 | PR-01–PR-05 preparation and WR-11; keep content within the allotted duration. |
| PR-07 | Prepare shared questions on the paper, architecture, metrics, baseline fairness, experimental findings, and limitations; rehearse answers and answer course Q&A. | All members | `docs/summary/qa.md`; practiced answers | Preparation: 09/12/2026; Q&A: course presentation date | Final report, slides, and PR-06; every member prepares to explain their contribution and answer questions. |
