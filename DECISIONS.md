# DECISIONS — Bayan

## Decision D-001 — Tokenizer & Max Length

* **Date:** 2026-09-30
* **Gate:** A
* **Status:** accepted
* **Owner:** Student

### Context

مقارنة Local WordPiece وmBERT على عينة صغيرة، باستخدام `fertility` و`truncation`.

### Decision

اعتماد **mBERT tokenizer** مع `max_length=12` كخط أساس.

* Arabic fertility: **2.89**
* English fertility: **1.57**
* Truncation: **40%**

سيتم إعادة تقييم `max_length` عند استخدام بيانات المشروع الفعلية.

### Evidence

* Day 1 Notebook 01
* Tokenization tests: **PASS**

### Consequences / Rollback

قد نرفع `max_length` إذا ظهرت نسبة truncation مرتفعة على البيانات الفعلية.

---

## Decision D-002 — Classification, NER & QA

* **Date:** 2026-09-30
* **Gate:** B
* **Status:** accepted
* **Owner:** Student

### Decision

اعتماد baseline **TF-IDF + LinearSVC** ومقارنة مع **Multilingual DistilBERT** باستخدام Partial Fine-tuning على CPU.

أفضل checkpoint للتصنيف: **Epoch 9** بناءً على Validation Macro-F1.

| Model                   | Val Macro-F1 | Test Macro-F1 |
| ----------------------- | -----------: | ------------: |
| TF-IDF + LinearSVC      |       0.6667 |        0.7333 |
| Multilingual DistilBERT |       1.0000 |        0.8667 |

Split:

* Train: **24**
* Validation: **8**
* Test: **8**
* `group_overlap = 0`

NER alignment:

* أول subword يحصل على label.
* continuation/special tokens = `-100`.
* Strict entity boundaries.

NER F1: **0.5714**

QA:

* no-answer → `None`
* reason → `no_answer_in_context`
* Valid span test: **PASS**
* No-answer test: **PASS**

### Evidence

* Lab 3 commit: `b5672665d06a2119c17f167ee0797194e4820128`
* Lab 4 commit: `5e3b7e50cff65c8eb54bc1382ad61951997576ea`
* `DAY2_NOTEBOOK3_CORE=PASS`
* `DAY2_NOTEBOOK4_CORE=PASS`

### Limitation

البيانات صغيرة ونتائجها **MEASURED_SMOKE**؛ لا تمثل أداءً إنتاجيًا.

---

## Decision D-003 — Arabic Preprocessing Profile

* **Date:** 2026-09-30
* **Gate:** C
* **Status:** accepted
* **Owner:** Student

### Decision

اعتماد **Two-Copy Contract**:

* `display_text`: النص الأصلي ولا يتم تغييره.
* `model_text`: نسخة معالجة للاستخدام في النموذج والبحث.

Profile:

`search/1.0.0`

ويشمل إزالة التشكيل و`tatweel`، وتوحيد الألف و`alef maksura`، مع الحفاظ على `taa marbuta`.

تم تثبيت:

`camel-tools==1.6.0`

Arabizi يتم التعامل معه كـ`passthrough`، والـheuristic ليس classifier.

### Evidence

* Golden tests: **4/4 PASS**
* `display_copy_preserved=True`
* `named_profile=True`
* `DAY3_NOTEBOOK5_CORE=PASS`
* `reports/bayan_arabic_profile.json`

مقارنة smoke على Gulf:

* Multilingual DistilBERT: **F1 = 0.0000**
* CAMeLBERT-DA: **F1 = 0.6667**

المقارنة على **4 أمثلة فقط وseed واحد**، ولا تثبت تفوقًا عامًا.

### Rollback

إصدار profile جديد إذا ظهرت مشاكل preprocessing على بيانات المشروع الفعلية.

---

## Decision D-004 — Bilingual Semantic Search

* **Date:** 2026-09-30
* **Gate:** C
* **Status:** accepted
* **Owner:** Student

### Decision

اعتماد:

* Encoder: `paraphrase-multilingual-MiniLM-L12-v2`
* Dimension: **384**
* Normalization: **L2**
* Index: **FAISS IndexFlatIP**
* `k = 3`

No-answer threshold تم ضبطه على validation فقط:

`0.4592`

ثم تم تجميده قبل test.

### Evidence

Test answerable:

* Recall@3: **1.0000**
* MRR@3: **0.6667**
* Queries: **6**

No-answer accuracy:

* Validation: **1.0000**
* Test: **1.0000**

Reranker:

`cross-encoder/mmarco-mMiniLMv2-L12-H384-v1`

* MRR قبل: **0.6667**
* MRR بعد: **0.7222**
* Delta: **+0.0556**
* Decision: `ADOPT_FOR_EXPERIMENT`

Evidence:

* `reports/search_manifest.json`
* `reports/retrieval_metrics.json`
* `DAY3_NOTEBOOK6_CORE=PASS`

### Limitation

النتائج **MEASURED_SMOKE** وعلى بيانات synthetic صغيرة.

---

## Decision D-005 — Evaluation & Error Analysis

* **Date:** 2026-09-30
* **Gate:** C
* **Status:** accepted
* **Owner:** Student

### Decision

اعتماد التقييم باستخدام:

* Macro-F1
* 95% Bootstrap CI
* Paired comparison
* Sliced evaluation
* Behavioural tests
* Manual error taxonomy

التقييم تم على **validation فقط** باستخدام `COURSE_FIXTURE`.

### Evidence

Macro-F1:

| Version |     F1 |        95% CI |
| ------- | -----: | ------------: |
| A       | 0.7807 | 0.6212–0.8982 |
| B       | 0.7819 | 0.6169–0.9042 |

Paired B−A:

* Difference: **+0.0012**
* 95% CI: **-0.1047 إلى +0.0996**
* Directional claim: **Not supported**

Behavioural tests:

* **3/6 PASS**
* Pass rate: **50%**

Taxonomy:

* `dialect_gap`: **3**
* `hard_or_ambiguous`: **3**
* `class_confusion`: **2**

### Ranked fixes

1. زيادة ومراجعة أمثلة Gulf للصحة والنقل.
2. إضافة contrastive examples لتقليل class confusion.
3. معالجة الطلبات القصيرة والملتبسة بإضافة context أو abstention عند الحاجة.

### Evidence

* `reports/day3_evaluation_fixture.json`
* `reports/day3_slice_report.csv`
* `reports/day3_error_taxonomy.csv`
* `DAY3_NOTEBOOK7_CORE=PASS`

### Limitation

النتائج مبنية على `COURSE_FIXTURE` صغيرة، لذلك لا تمثل أداء المشروع الحقيقي أو الإنتاج.

---

# قرارات إلزامية قبل Gate E

* [x] tokenizer + max length
* [x] Arabic preprocessing profile
* [x] task model/baseline and split
* [x] semantic encoder/index/k/threshold
* [x] metric/slices/error priorities
* [ ] performance budget
* [ ] ONNX/INT8 adopt or reject
* [ ] served artefact + preprocessing/label versions
