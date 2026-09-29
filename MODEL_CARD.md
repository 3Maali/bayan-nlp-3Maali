# بطاقة نموذج بيان | Bayan Model Card

> جميع النتائج الحالية `MEASURED_SMOKE` أو `COURSE_FIXTURE` وليست benchmark إنتاجيًا.

---

# 1. Topic Classification

## Model details

* Name/version: `Bayan Topic Classifier / Day 2`
* Base checkpoint: `Multilingual DistilBERT`
* Task: `Text Classification`
* License/source: `Hugging Face model checkpoint / course fixture`
* Commit SHA: `b5672665d06a2119c17f167ee0797194e4820128`
* Owner/contact role: `Student`

## Intended use

* الاستخدام المقصود: تصنيف النصوص إلى الفئات المحددة في بيانات التدريب.
* المستخدمون المقصودون: فرق NLP والتقييم.
* خارج النطاق: الاستخدام الإنتاجي أو القرارات عالية الأثر دون تقييم إضافي.

## Data and preprocessing

* Dataset ID/version: `Bayan Day 2 classification fixture`
* Languages/variants: `Arabic / English`
* Split strategy: `Train 24 / Validation 8 / Test 8; group_overlap=0`
* PII policy: `Synthetic/course data only`
* Preprocessing profile/version/backend: `Day 1 preprocessing`
* Tokenizer/embedding model: `mBERT tokenizer`

## Evaluation

| metric/slice        |  n |   result | uncertainty      | evidence file |
| ------------------- | -: | -------: | ---------------- | ------------- |
| Test Macro-F1       |  8 | `0.8667` | Single smoke run | Day 2 Lab 3   |
| Test Accuracy       |  8 | `0.8750` | Single smoke run | Day 2 Lab 3   |
| Validation Macro-F1 |  8 | `1.0000` | Single smoke run | Day 2 Lab 3   |

## Behavioural checks

| capability                    | pass rate | known failure   |
| ----------------------------- | --------: | --------------- |
| Classification split contract |    `PASS` | None in fixture |

## Limitations and risks

1. البيانات صغيرة ومصطنعة.
2. لا يوجد دليل كافٍ على التعميم على بيانات إنتاجية.
3. لم يتم قياس الاستقرار عبر عدة seeds.

## Ethical and privacy notes

* البيانات الحالية course/synthetic ولا تحتوي PII إنتاجية.
* لا توجد claims إنتاجية.

## Reproduction

1. افتح notebook: `notebooks/03_text_classification.ipynb`.
2. استخدم runtime/device: `Google Colab / CPU`.
3. ثبت النسخ المطلوبة في notebook.
4. شغّل Run all من commit: `b5672665d06a2119c17f167ee0797194e4820128`.
5. قارن النتيجة مع نتائج Day 2 classification.

---

# 2. NER

## Model details

* Name/version: `Bayan NER / Day 2`
* Base checkpoint: `Multilingual Transformer`
* Task: `Named Entity Recognition`
* License/source: `Course fixture`
* Commit SHA: `5e3b7e50cff65c8eb54bc1382ad61951997576ea`
* Owner/contact role: `Student`

## Intended use

* الاستخدام المقصود: استخراج الكيانات المحددة في النص.
* المستخدمون المقصودون: فرق NLP والتقييم.
* خارج النطاق: استخراج كيانات عامة من بيانات إنتاجية دون إعادة تقييم.

## Data and preprocessing

* Dataset ID/version: `Bayan Day 2 NER fixture`
* Languages/variants: `Arabic / English`
* Split strategy: `Course fixture`
* PII policy: `Synthetic/course data only`
* Preprocessing profile/version/backend: `Subword alignment`
* Tokenizer/embedding model: `Transformer tokenizer`

## Evaluation

| metric/slice     |       n |   result | uncertainty      | evidence file |
| ---------------- | ------: | -------: | ---------------- | ------------- |
| Strict entity F1 | Fixture | `0.5714` | Single smoke run | Day 2 Lab 4   |

## Behavioural checks

| capability        | pass rate | known failure                                |
| ----------------- | --------: | -------------------------------------------- |
| Subword alignment |    `PASS` | Continuation/special tokens mapped to `-100` |

## Limitations and risks

1. Strict entity F1 = `0.5714` على عينة صغيرة.
2. النتائج لا تمثل بيانات إنتاجية.
3. حدود الكيانات تحتاج تقييمًا أوسع.

## Ethical and privacy notes

* Course/synthetic data only.
* لا يوجد استخدام لبيانات PII إنتاجية.

## Reproduction

1. افتح notebook: `notebooks/04_ner_and_qa.ipynb`.
2. استخدم runtime/device: `Google Colab / CPU`.
3. ثبت النسخ المطلوبة.
4. شغّل Run all من commit: `5e3b7e50cff65c8eb54bc1382ad61951997576ea`.
5. قارن مع نتيجة Strict entity F1 أعلاه.

---

# 3. Extractive QA

## Model details

* Name/version: `Bayan Extractive QA / Day 2`
* Base checkpoint: `Multilingual Transformer`
* Task: `Extractive Question Answering`
* License/source: `Course fixture`
* Commit SHA: `5e3b7e50cff65c8eb54bc1382ad61951997576ea`
* Owner/contact role: `Student`

## Intended use

* الاستخدام المقصود: استخراج الإجابة من السياق عند وجودها.
* المستخدمون المقصودون: فرق NLP والتقييم.
* خارج النطاق: الإجابة خارج السياق أو الاستخدام الإنتاجي دون benchmark.

## Data and preprocessing

* Dataset ID/version: `Bayan Day 2 QA fixture`
* Languages/variants: `Arabic / English`
* Split strategy: `Course fixture`
* PII policy: `Synthetic/course data only`
* Preprocessing profile/version/backend: `QA span alignment`
* Tokenizer/embedding model: `Transformer tokenizer`

## Evaluation

| metric/slice    |       n | result | uncertainty | evidence file |
| --------------- | ------: | -----: | ----------- | ------------- |
| Valid span test | Fixture | `PASS` | Smoke test  | Day 2 Lab 4   |
| No-answer test  | Fixture | `PASS` | Smoke test  | Day 2 Lab 4   |

## Behavioural checks

| capability          | pass rate | known failure   |
| ------------------- | --------: | --------------- |
| No-answer handling  |    `PASS` | None in fixture |
| Boundary validation |    `PASS` | None in fixture |

## Limitations and risks

1. لم يتم الحصول على benchmark إنتاجي.
2. العينة صغيرة.
3. تحتاج الإجابات وحدودها إلى تقييم أوسع.

## Ethical and privacy notes

* Course/synthetic data only.
* لا توجد PII إنتاجية.

## Reproduction

1. افتح notebook: `notebooks/04_ner_and_qa.ipynb`.
2. استخدم runtime/device: `Google Colab / CPU`.
3. ثبت النسخ المطلوبة.
4. شغّل Run all من commit: `5e3b7e50cff65c8eb54bc1382ad61951997576ea`.
5. قارن نتائج QA مع اختبارات الـspan وno-answer.

---

# 4. Bilingual Embeddings / Semantic Search

## Model details

* Name/version: `Bayan Semantic Search / 1.0.0`
* Base checkpoint: `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`
* Task: `Bilingual Semantic Retrieval`
* License/source: `Hugging Face / Sentence Transformers`
* Commit SHA: `39adb418c6e74c387ce243c5b35663ff837ab582`
* Owner/contact role: `Student`

## Intended use

* الاستخدام المقصود: البحث الدلالي العربي والإنجليزي.
* المستخدمون المقصودون: تطبيقات البحث والتجارب الداخلية.
* خارج النطاق: benchmark إنتاجي أو استخدامه كمرجع وحيد للقرارات.

## Data and preprocessing

* Dataset ID/version: `bayan_day3_cases.csv`
* Languages/variants: `Arabic / English`
* Split strategy: `Validation 10 / Test 8 queries`
* PII policy: `Synthetic/course data only`
* Preprocessing profile/version/backend: `Arabic search/1.0.0 + English NFC-whitespace/1.0.0`
* Tokenizer/embedding model: `paraphrase-multilingual-MiniLM-L12-v2`

## Evaluation

| metric/slice           |  n |   result | uncertainty      | evidence file                    |
| ---------------------- | -: | -------: | ---------------- | -------------------------------- |
| Recall@3               |  6 | `1.0000` | Single smoke run | `reports/retrieval_metrics.json` |
| MRR@3                  |  6 | `0.6667` | Single smoke run | `reports/retrieval_metrics.json` |
| Cross-lingual Recall@3 |  2 | `1.0000` | SMALL_SLICE      | `reports/retrieval_metrics.json` |
| Cross-lingual MRR@3    |  2 | `0.5000` | SMALL_SLICE      | `reports/retrieval_metrics.json` |

## Behavioural checks

| capability           |             pass rate | known failure               |
| -------------------- | --------------------: | --------------------------- |
| No-answer validation |              `1.0000` | None in fixture             |
| No-answer test       |              `1.0000` | None in fixture             |
| Reranking            | `MRR 0.6667 → 0.7222` | CPU latency not benchmarked |

## Limitations and risks

1. النتائج مبنية على synthetic data.
2. بعض الشرائح صغيرة جدًا.
3. latency وthroughput لم يتم قياسهما بعد.

## Ethical and privacy notes

* Synthetic/course data only.
* لا توجد PII إنتاجية.

## Reproduction

1. افتح notebook: `notebooks/06_semantic_search.ipynb`.
2. استخدم runtime/device: `Google Colab / CPU`.
3. ثبت النسخ: `sentence-transformers 6.0.0`, `faiss-cpu 1.15.0`, `camel-tools 1.6.0`.
4. شغّل Run all من commit: `39adb418c6e74c387ce243c5b35663ff837ab582`.
5. قارن النتائج مع `reports/retrieval_metrics.json`.

---

# 5. Reranker

## Model details

* Name/version: `Bayan Reranker / Experimental`
* Base checkpoint: `cross-encoder/mmarco-mMiniLMv2-L12-H384-v1`
* Task: `Candidate Reranking`
* License/source: `Hugging Face / Cross-Encoder`
* Commit SHA: `39adb418c6e74c387ce243c5b35663ff837ab582`
* Owner/contact role: `Student`

## Intended use

* الاستخدام المقصود: إعادة ترتيب نتائج البحث بعد الاسترجاع الأولي.
* المستخدمون المقصودون: تجارب البحث الدلالي.
* خارج النطاق: اعتماد إنتاجي قبل قياس latency وbenchmark أكبر.

## Data and preprocessing

* Dataset ID/version: `bayan_day3_cases.csv`
* Languages/variants: `Arabic / English`
* Split strategy: `Validation threshold tuning + frozen test`
* PII policy: `Synthetic/course data only`
* Preprocessing profile/version/backend: `Arabic search/1.0.0 + English NFC-whitespace/1.0.0`
* Tokenizer/embedding model: `cross-encoder/mmarco-mMiniLMv2-L12-H384-v1`

## Evaluation

| metric/slice           |  n |    result | uncertainty      | evidence file                    |
| ---------------------- | -: | --------: | ---------------- | -------------------------------- |
| MRR@3 before reranking |  6 |  `0.6667` | Single smoke run | `reports/retrieval_metrics.json` |
| MRR@3 after reranking  |  6 |  `0.7222` | Single smoke run | `reports/retrieval_metrics.json` |
| Delta                  |  6 | `+0.0556` | Single smoke run | `reports/retrieval_metrics.json` |

## Behavioural checks

| capability          |  pass rate | known failure               |
| ------------------- | ---------: | --------------------------- |
| Candidate reranking | `MEASURED` | CPU latency not benchmarked |

## Limitations and risks

1. النتيجة مبنية على 6 answerable test queries.
2. لا يوجد benchmark latency إنتاجي.
3. القرار الحالي `ADOPT_FOR_EXPERIMENT` وليس اعتمادًا إنتاجيًا.

## Ethical and privacy notes

* Synthetic/course data only.
* لا توجد PII إنتاجية.

## Reproduction

1. افتح notebook: `notebooks/06_semantic_search.ipynb`.
2. استخدم runtime/device: `Google Colab / CPU`.
3. ثبت النسخ المطلوبة.
4. شغّل Run all من commit: `39adb418c6e74c387ce243c5b35663ff837ab582`.
5. قارن مع `reports/retrieval_metrics.json`.
