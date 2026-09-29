# Md. Rafi

AI engineer, Dhaka. I build LLM systems and the evaluation harnesses that tell you whether they actually work.

Most of what I know, I know because I measured something and it disagreed with me. Three of the projects below ended that way, and each one is published with the numbers that killed it.

---

## Where the measurement disagreed with me

**[retrieval-eval](https://github.com/MdRaf1/the-data-guardian) — three negative results**

I built a retrieval evaluation harness to test claims one of my own earlier designs had asserted with nothing behind them. 300-document corpus, 36 graded queries frozen before a single retriever ran, seven retrievers scored on nDCG@10, Recall@10 and MRR@10.

Three things did not work, each with its mechanism. Title boosting came out bit-identical to the baseline on paraphrase. RRF hybrid fusion landed strictly between its two legs on every subset, because rank-only fusion imports a starved lexical candidate list at full weight. And cross-encoder re-ranking was net flat against dense retrieval — 0.622 against 0.623 nDCG@10 — while costing recall.

Nothing deployed. No users. The output is the finding.

**[guardian-analytics](https://github.com/MdRaf1/guardian-analytics) — three indexes built, measured, dropped**

A normalised five-table PostgreSQL model with 201,619 synthetic rows, exposed as a read-only API I deployed and still operate: readiness endpoints, structured logs, uptime alerting, a public status page, a CI gate asserting query counts against the live database on every push, and a daily 26-check data-quality job.

I picked its indexes by measurement — EXPLAIN (ANALYZE, BUFFERS), medians of five runs — and dropped three candidates with the numbers that disqualified them.

Then I dropped a measured index on the live database on purpose, because I had never watched the system without one and did not actually know whether my monitoring would notice. The repeat-offender query went 424 ms → 592 ms. The cause was not the missing lookup: the window sort had lost its pre-sorted input and was spilling 5,144 kB to disk. I restored it and wrote it up.

All load on it is synthetic — a generator, CI, and the uptime monitor. There are no real users.

**[llm-lab](https://github.com/MdRaf1/llm-lab) — I disproved my own premise, then my own headline**

Could a GPU-less commodity PC run models far larger than its RAM by streaming expert weights from disk? I measured it. No: on that box decode is bound by compute and RAM bandwidth, not disk.

Then a better measurement disproved my own headline figure — 4.3 tok/s turned out to be a per-invocation cold-load artefact, against roughly 9.2 warm and sustained.

It ships a bit-exact oracle that refuses to compare runs whose configuration fingerprints differ, and five reporting rules I hold myself to: no number appears anywhere unless the command that produced it and its output are committed.

[**The bottleneck wasn't the disk**](https://github.com/MdRaf1/llm-lab/blob/master/docs/exact-moe-on-commodity-hardware.md) — the write-up.

---

## Things that run

- **[Git Sensei](https://github.com/MdRaf1/Git-sensei)** — Python CLI on **PyPI**. Translates intent into Git commands, with a guard layer that intercepts destructive operations before they execute. Built on the OpenRouter API.
- **[Baseline Guardian](https://github.com/MdRaf1/baseline-guardian)** — TypeScript action on the **GitHub Marketplace**. Scans CSS against MDN Baseline data on every pull request and blocks non-compliant merges, rather than filing a warning nobody has to act on.
- **[SessionSolve](https://github.com/MdRaf1/SessionSolve)** — multimodal debugging agent on **Hugging Face Spaces**. Takes a screen recording of a bug plus the repository, localises the fault, and emits a generated Playwright regression test — an executable check, not a written diagnosis.
- **[The Data Guardian](https://github.com/MdRaf1/the-data-guardian)** — autonomous privacy agent on Elastic Cloud. An ES|QL tool pre-filters; the model judges exposure against retrieved policy rather than regex; a separate Workflow performs the redaction, so the agent itself never holds write access to the index.
- **[Teletext Zero](https://github.com/MdRaf1/Teletext-Zero)** — live news and weather rendered inside a strict 40×24 Teletext grid. As much a constraint problem as a UI one.

---

## In other people's codebases

Three patches reviewed and merged upstream in projects I did not write:

- a version probe reading a nonzero exit as "not installed", silently disabling a working extractor;
- a page-break rule wrong across seven document templates — submitted with a regression test, so the corrected behaviour could not silently revert;
- a build fix on **HackMatrix**, a C++ 3D Linux desktop environment: diagnosed Abseil missing-symbol linker errors and corrected the makefile link flags to restore a broken build.

---

## Currently

Software engineering undergraduate. Dhaka, Bangladesh (UTC+6), moving to Kuala Lumpur (UTC+8) in Q4 2026. Available for remote work now.

Python · TypeScript · SQL · C · PostgreSQL · Node.js · Playwright · GitHub Actions

[rafiautomation.systems](https://rafiautomation.systems) · [LinkedIn](https://www.linkedin.com/in/md-raf1) · hello@rafiautomation.systems
