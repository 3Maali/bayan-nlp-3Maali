# Bayan — Bilingual Applied NLP Project

**Learner ID / GitHub username:** 3Maali
**GitHub:** https://github.com/3Maali/bayan-nlp-3Maali
**Final release:** `submission-v1.0`

## Executive summary | الملخص

Bayan is an educational bilingual Arabic/English NLP project that applies text preprocessing, topic and sentiment classification, NER, extractive QA, semantic search, evaluation, and optimized serving. The project uses small educational datasets and public/synthetic examples to demonstrate an end-to-end NLP workflow. The project is intended for learning and experimentation, not as a production system. The data used in this project are educational, synthetic/public examples and are not real beneficiary data.

## What Bayan does | ماذا يفعل بيان؟

1. **Privacy and preprocessing:** preserves original text separately from model-ready text and applies Arabic/English preprocessing with basic PII masking and normalization.
2. **Topic and sentiment classification:** provides separate topic and sentiment classification heads using multilingual transformer-based modeling.
3. **NER:** performs token-level named entity recognition with subword-label alignment.
4. **Extractive QA:** extracts answers from context and supports a no-answer outcome when the answer is not present.
5. **Bilingual semantic search:** retrieves relevant Arabic/English text using multilingual embeddings, FAISS, and optional reranking.
6. **Evaluation and serving:** evaluates model behaviour and error slices, benchmarks optimized inference, and exposes a FastAPI serving interface with bilingual canaries.

## Scope and non-goals | النطاق وما لا يدعيه المشروع

* **In scope:** bilingual Arabic/English NLP experiments, preprocessing, classification, NER, extractive QA, semantic search, evaluation, optimization, and serving.
* **Out of scope:** production-scale training, large-scale deployment, real beneficiary data, high-stakes automated decisions, and claims of general performance from the small educational datasets.
* **Not for:** production/government decisions without further validation — including larger representative datasets, security/privacy review, domain validation, monitoring, and human oversight.

## Reproduce on Google Colab Free

| **#** | **Notebook**                 | **Colab**                                                                                                                                   | **Purpose** |
| ----- | ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
| 00    | runtime doctor               | [Open in Colab](https://colab.research.google.com/github/3Maali/bayan-nlp-3Maali/blob/main/notebooks/00_runtime_doctor.ipynb)               | environment |
| 01    | text processing/tokenisation | [Open in Colab](https://colab.research.google.com/github/3Maali/bayan-nlp-3Maali/blob/main/notebooks/01_text_processing_tokenization.ipynb) | Gate A      |
| 02    | attention/transformers       | [Open in Colab](https://colab.research.google.com/github/3Maali/bayan-nlp-3Maali/blob/main/notebooks/02_attention_transformers.ipynb)       | LO2         |
| 03    | classification               | [Open in Colab](https://colab.research.google.com/github/3Maali/bayan-nlp-3Maali/blob/main/notebooks/03_classification.ipynb)               | Gate B      |
| 04    | NER and QA                   | [Open in Colab](https://colab.research.google.com/github/3Maali/bayan-nlp-3Maali/blob/main/notebooks/04_ner_and_qa.ipynb)                   | Gate B      |
| 05    | Arabic NLP                   | [Open in Colab](https://colab.research.google.com/github/3Maali/bayan-nlp-3Maali/blob/main/notebooks/05_arabic_nlp.ipynb)                   | Gate C      |
| 06    | semantic search              | [Open in Colab](https://colab.research.google.com/github/3Maali/bayan-nlp-3Maali/blob/main/notebooks/06_semantic_search.ipynb)              | Gate C      |
| 07    | evaluation/error analysis    | [Open in Colab](https://colab.research.google.com/github/3Maali/bayan-nlp-3Maali/blob/main/notebooks/07_evaluation_error_analysis.ipynb)    | Gate C      |
| 08    | optimisation/serving         | [Open in Colab](https://colab.research.google.com/github/3Maali/bayan-nlp-3Maali/blob/main/notebooks/08_optimization_serving.ipynb)         | Gate D      |

**Note:** If your actual notebook filenames differ, update only the links/names in this table before the final release.

### Clean-run instructions

1. Open notebook 00 and choose **Save a copy in Drive**.
2. Run in numeric order using Colab Free.
3. Use **Runtime → Restart session and run all** before final evidence.
4. Do not place tokens, PII, model weights, or private Drive links in the repository.

## Architecture

```text
                 ┌──────────────────────┐
                 │ Arabic / English Text│
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │ Privacy + Preprocess │
                 │ raw_text / model_text│
                 └──────────┬───────────┘
                            │
             ┌──────────────┼──────────────┐
             ▼              ▼              ▼
       Classification      NER        Semantic Search
       ├─ Topic            │          ├─ Embeddings
       └─ Sentiment        │          ├─ FAISS
                            │          └─ Reranking
             │              │
             └──────┬───────┘
                    ▼
             Extractive QA
             + no-answer
                    │
                    ▼
          Evaluation + Error Analysis
                    │
                    ▼
            Optimized Serving
          PyTorch → ONNX FP32
```

## Results | النتائج

كل رقم يحمل `MEASURED`, `MEASURED_SMOKE`, `SYSTEMS_SMOKE`, `TARGET`, أو `REFERENCE`.

| **Component**            | **Metric**                     | **Result + label**                                          | **Split/workload**                      | **Evidence**         |
| ------------------------ | ------------------------------ | ----------------------------------------------------------- | --------------------------------------- | -------------------- |
| topic classification     | Macro-F1                       | **0.8667 — MEASURED_SMOKE**                                 | test, n=8                               | `DECISIONS.md` D-002 |
| sentiment classification | Macro-F1                       | **Not reported as a separate Lab 8 benchmark — REFERENCE**  | separate classification head            | `DECISIONS.md` D-002 |
| NER                      | entity F1                      | **0.5714 — MEASURED_SMOKE**                                 | Day 2 fixture                           | `DECISIONS.md` D-002 |
| QA                       | EM/F1/no-answer                | **No production-style aggregate reported — MEASURED_SMOKE** | Day 2 QA fixture                        | `DECISIONS.md` D-002 |
| search                   | Recall@3 / MRR@3               | **1.0000 / 0.6667 — MEASURED_SMOKE**                        | test, n=6 answerable                    | `DECISIONS.md` D-004 |
| serving                  | p95 / throughput / quality tax | **340.68 ms / 30.99 items/s / 0.0000 — MEASURED**           | Project Artifact, model-only, ONNX FP32 | `BENCHMARKS.md`      |

### Classification details

The topic classification model achieved validation Macro-F1 of **1.0000** and test Macro-F1 of **0.8667** on the small educational fixture. These results are reported as smoke measurements and should not be interpreted as production performance.

Topic and sentiment are separate classification heads. The Lab 8 Project Artifact benchmark evaluates the **topic** head only.

### Semantic search details

The semantic search test achieved Recall@3 of **1.0000** and MRR@3 of **0.6667** on six answerable test examples. A cross-encoder reranker improved MRR@3 from **0.6667 to 0.7222** on the same small experiment.

## Error found and decision | خطأ وقرار

* **Observed failure:** errors were concentrated in Gulf-dialect examples, ambiguous short requests, and confusion between closely related classes.
* **Slice/taxonomy:** `dialect_gap` = 3 errors, `hard_or_ambiguous` = 3 errors, `class_confusion` = 2 errors.
* **Fix or deferred action:** increase/refine Gulf examples, add contrastive examples for confusing classes, and improve handling of short ambiguous requests with context or abstention.
* **Evidence after change:** `reports/day3_slice_report.csv` and `reports/day3_error_taxonomy.csv`. The fixes are documented as follow-up actions rather than evidence of a new production-quality benchmark.

## Measured extension | الامتداد المقاس

* **Extension chosen:** ONNX FP32 optimized serving with dynamic INT8 evaluation.
* **Baseline:** PyTorch FP32 model-only inference on the same bilingual project workload.
* **Benefit/cost metric:** ONNX FP32 achieved 340.68 ms p95 and 30.99 items/s, compared with 394.44 ms p95 and 27.80 items/s for PyTorch. Prediction agreement was 1.0 and quality tax was 0.0.
* **Evidence path:** `BENCHMARKS.md` and Lab 8 optimization/serving notebook.
* **Decision and limitation:** `ADOPT_ONNX_FP32`. Dynamic INT8 was evaluated but not selected because its quality tax was **0.5655**, despite lower latency and smaller model size. The quality comparison is based on the small validation workload of eight examples.

## Repository evidence

* `DATA_CARD.md`
* `MODEL_CARD.md`
* `EVALUATION_REPORT.md`
* `BENCHMARKS.md`
* `DECISIONS.md`
* `PROGRESS.md`
* `PROJECT_SUMMARY.json`
* `SUBMISSION.yml`

## Limitations and responsible use

* **Data limitation:** the project uses small educational/public or synthetic examples and does not represent real beneficiary populations or production traffic.
* **Arabic/dialect/Arabizi limitation:** Arabic dialect variation, Gulf Arabic, Arabizi, short text, and ambiguous wording remain challenging.
* **Task/model limitation:** the transformer and search experiments are educational smoke tests with small datasets and should not be treated as production benchmarks.
* **Evaluation uncertainty:** several reported metrics use small evaluation sets, so uncertainty is high and results may change with a larger representative dataset.
* **Serving/security limitation:** the serving benchmark was measured on Colab CPU. It does not establish production capacity, security, availability, or operational reliability.
* **Human review requirement:** outputs should be reviewed by qualified users before being used for consequential decisions.

## Final validation

```bash
PYTHONPATH=src python scripts/validate_submission.py . --require-tag
PYTHONPATH=src python scripts/preflight_submission.py . --require-tag
```

* **Validator status:** `BAYAN_SUBMISSION_VALIDATOR=PASS` after final validation.
* **CI badge/link:** Not configured.
* **Release `submission-v1.0`:** Final tag to be created after all Gate E checks pass.

## Presentation | العرض

See `PRESENTATION.md`. Add the final presentation link and selected project examples before the final hand-in.

## My contribution | مساهمتي

* **My change and file:** implemented and documented the bilingual NLP workflow across the project notebooks and repository evidence, including preprocessing, classification, NER/QA, Arabic NLP, semantic search, evaluation, and optimized serving.
* **Reason and evidence:** the project demonstrates an end-to-end educational NLP workflow with reproducible decisions, measured evaluation, error analysis, and a serving benchmark. Evidence is recorded in `DECISIONS.md`, `EVALUATION_REPORT.md`, `MODEL_CARD.md`, and `BENCHMARKS.md`.

## AI assistance | الاستعانة بالأدوات

AI tools were used as development assistance for explaining concepts, reviewing code, troubleshooting implementation issues, and helping structure documentation. Project results were executed and verified by the learner in the project environment. Final decisions, measurements, code execution, and repository contents were reviewed by the learner.

## Training context | السياق التدريبي

This educational project was developed during Applied Natural Language Processing with Transformers (SDA-AIE-211) in the SDAIA Academy training context. أُنجز هذا المشروع التعليمي ضمن دورة معالجة اللغات الطبيعية باستخدام المحولات (SDA-AIE-211) في السياق التدريبي لأكاديمية سدايا.

Academy | الأكاديمية: [SDAIA Academy](https://github.com/SDAIAAcademy)
Trainer | المدربة: Meaad Al-Marri — ميعاد المري
Course source: https://github.com/almiyead-rgb/bayan-applied-nlp-course
#SDAIAAcademy

This attribution does not claim Academy endorsement or ownership of third-party assets. لا يدعي هذا النسب اعتماد المشروع أو تملك أصول الأطراف الأخرى.

## Final hand-in acknowledgement | إقرار التسليم النهائي

I confirm that I reviewed the project requirements, repository evidence, validation results, privacy checks, and final release requirements. I understand that this version is the graded submission and that the final tag is `submission-v1.0`.

أقر بأنني راجعت متطلبات المشروع وأدلة المستودع ونتائج التحقق ومتطلبات الخصوصية والتسليم النهائي، وأفهم أن هذه النسخة هي نسخة التسليم للتقييم وأن الوسم النهائي هو `submission-v1.0`.

## License and acknowledgements

Project code is provided for educational purposes. Third-party libraries, pretrained models, datasets, and institutional marks remain subject to their respective licenses and terms. This project does not claim ownership of third-party models, libraries, datasets, or institutional marks.

Pretrained models and libraries used in the project include multilingual transformer models, Sentence Transformers, FAISS, PyTorch, ONNX/ONNX Runtime, FastAPI, and related open-source dependencies. Their original licenses and source documentation should be consulted before reuse or redistribution.
