# PROGRESS — Bayan Gates A–E

**Student GitHub:** 3Maali
**Repository:** bayan-nlp-3Maali
**Last updated:** 2026-09-30

لا تضع علامة ✅ قبل وجود رابط commit/report/test قابل للفحص.

| Gate               | Status         | Required evidence                                                            | Commit/report links                                                                                                                                                                                                                                                                                             | Blocker/next action                                |
| ------------------ | -------------- | ---------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------- |
| A — ingest         | ✅ PASSED       | preprocessing tests + tokenizer decision                                     | [Day 1 commit](https://github.com/3Maali/bayan-nlp-3Maali/commit/049bb67475346712550748b64db123f102d98483)                                                                                                                                                                                                      | مكتمل                                              |
| B — tasks          | ✅ PASSED       | classification + NER + QA evidence                                           | [Lab 3](https://github.com/3Maali/bayan-nlp-3Maali/commit/b5672665d06a2119c17f167ee0797194e4820128) + [Lab 4](https://github.com/3Maali/bayan-nlp-3Maali/commit/5e3b7e50cff65c8eb54bc1382ad61951997576ea)                                                                                                       | مكتمل                                              |
| C — search & truth | ✅ PASSED       | Arabic profile + search metrics + slices + taxonomy + evaluation/model cards | [Lab 5](https://github.com/3Maali/bayan-nlp-3Maali/commit/850090386e677bd8ac511d599a7ac603ed61ba03) + [Lab 6](https://github.com/3Maali/bayan-nlp-3Maali/commit/39adb418c6e74c387ce243c5b35663ff837ab582) + [Lab 7](https://github.com/3Maali/bayan-nlp-3Maali/commit/5b41624bb6d942d1936cd7968a2a790a9a1007c7) | مكتمل                                              |
| D — ship           | ✅ PASSED       | project benchmark + API tests + canaries + optimization evidence             | Final Notebook + `reports/benchmark_results.json` + `reports/service_smoke.json` + `BENCHMARKS.md` + `DECISIONS.md`                                                                                                                                                                                             | مكتمل                                              |
| E — submit         | 🟨 IN_PROGRESS | validator + demo + release tag                                               | FILL_ME                                                                                                                                                                                                                                                                                                         | تشغيل validator النهائي ثم إنشاء `submission-v1.0` |

Status values: `⬜ NOT_STARTED`, `🟨 IN_PROGRESS`, `✅ PASSED`, `🟥 BLOCKED`.

## Runtime/run-all evidence

| Notebook | Clean run date | Core marker                | Colab/GitHub link                                                                                             |
| -------- | -------------- | -------------------------- | ------------------------------------------------------------------------------------------------------------- |
| 00       | 2026-09-30     | `runtime checks`           | [Notebook 00](https://github.com/3Maali/bayan-nlp-3Maali/blob/main/notebooks/00_runtime_doctor.ipynb)         |
| 01       | 2026-09-30     | `DAY1_NOTEBOOK1_CORE=PASS` | [Lab 1](https://github.com/3Maali/bayan-nlp-3Maali/blob/main/notebooks/01_text_processing_tokenization.ipynb) |
| 02       | 2026-09-30     | `DAY1_NOTEBOOK2_CORE=PASS` | [Lab 2](https://github.com/3Maali/bayan-nlp-3Maali/blob/main/notebooks/02_attention_transformers.ipynb)       |
| 03       | 2026-09-30     | `DAY2_NOTEBOOK3_CORE=PASS` | [Lab 3](https://github.com/3Maali/bayan-nlp-3Maali/commit/b5672665d06a2119c17f167ee0797194e4820128)           |
| 04       | 2026-09-30     | `DAY2_NOTEBOOK4_CORE=PASS` | [Lab 4](https://github.com/3Maali/bayan-nlp-3Maali/commit/5e3b7e50cff65c8eb54bc1382ad61951997576ea)           |
| 05       | 2026-09-30     | `DAY3_NOTEBOOK5_CORE=PASS` | [Lab 5](https://github.com/3Maali/bayan-nlp-3Maali/commit/850090386e677bd8ac511d599a7ac603ed61ba03)           |
| 06       | 2026-09-30     | `DAY3_NOTEBOOK6_CORE=PASS` | [Lab 6](https://github.com/3Maali/bayan-nlp-3Maali/commit/39adb418c6e74c387ce243c5b35663ff837ab582)           |
| 07       | 2026-09-30     | `DAY3_NOTEBOOK7_CORE=PASS` | [Lab 7](https://github.com/3Maali/bayan-nlp-3Maali/commit/5b41624bb6d942d1936cd7968a2a790a9a1007c7)           |
| 08       | 2026-09-30     | `DAY4_NOTEBOOK8_CORE=PASS` | [Lab 8](https://github.com/3Maali/bayan-nlp-3Maali/commit/461ca0fd89ff47b34305081c9038e845fa1be210)           |

> **Final project evidence:**
> The final `PROJECT_ARTIFACT` benchmark and serving validation were completed in `Final_NLP.ipynb`. The Notebook 08 commit above records the optimization/serving lab implementation; the final project benchmark is documented separately in `BENCHMARKS.md` and `reports/benchmark_results.json`.

## Final release

* Final commit: FILL_ME
* Release/tag `submission-v1.0`: FILL_ME
* Validator pre-tag report: FILL_ME
* Validator `--require-tag` report: FILL_ME
* Private-window visibility check: PASS / FAIL — FILL_ME
* Remaining limitation: `Final submission validation and release tagging are still pending.`
