# DECISIONS — Bayan

> انسخ القالب إلى `DECISIONS.md`. أضف قرارًا جديدًا لكل تغيير يؤثر في البيانات أو الجودة أو الخدمة.

## Decision D-001 — اختيار الـTokenizer وMax Length

* **Date:** 2026-09-30
* **Gate:** A
* **Status:** accepted
* **Owner:** Student

### Context | السياق

المشروع يحتاج إلى معالجة نصوص عربية وإنجليزية باستخدام نموذج ثنائي اللغة. تمت مقارنة سلوك الـtokenization على عينة صغيرة من 5 نصوص صناعية، مع قياس `fertility` و`truncation`.

تم تثبيت العينة المستخدمة في القياس، ولم يتم تغيير بياناتها أثناء المقارنة.

### Options considered | البدائل

| **Option**      | **Benefit**                                                                     | **Cost/risk**                                                                | **Evidence**                                                        |
| --------------- | ------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| Local WordPiece | بسيط ومفيد للتعلم وفهم آلية تقسيم النص إلى subwords                             | Tokenizer تعليمي وليس مرتبطًا بنموذج pretrained                              | Arabic fertility = 1.39، English fertility = 1.32، truncation = 0%  |
| mBERT tokenizer | متوافق مباشرة مع نموذج `bert-base-multilingual-cased` ويدعم العربية والإنجليزية | Arabic fertility أعلى في العينة الحالية، وحدث truncation عند `max_length=12` | Arabic fertility = 2.89، English fertility = 1.57، truncation = 40% |

### Decision | القرار

تم اختيار **mBERT tokenizer** مع `max_length=12` كخط أساس للمشروع، لأنه متوافق مباشرة مع النموذج pretrained المختار ويدعم معالجة النصوص العربية والإنجليزية.

تم اعتماد `max_length=12` كقيمة أولية في هذه المرحلة، مع تسجيل أن نسبة الـtruncation بلغت 40% على العينة الحالية. سيتم إعادة تقييم هذه القيمة عند استخدام نصوص المشروع الفعلية.

### Evidence | الدليل

* **Report/test/commit:** Day 1 Notebook 01 — Text Processing & Tokenisation
* **Metric and result label:** mBERT Arabic fertility = 2.89، English fertility = 1.57، truncation rate @ `max_length=12` = 40%
* **Slice or failure considered:** 5 نصوص صناعية عربية وإنجليزية، مع ملاحظة حدوث truncation في 40% من العينة عند `max_length=12`.

### Consequences and rollback | الأثر والرجوع

* **Positive consequence:** استخدام tokenizer متوافق مباشرة مع mBERT ويدعم اللغتين المطلوبتين في المشروع.
* **Limitation/new risk:** ارتفاع Arabic fertility ووجود truncation عند `max_length=12` على العينة الحالية قد يؤثران على كفاءة المعالجة للنصوص الأطول.
* **Rollback trigger:** إذا أظهرت بيانات المشروع الفعلية نسبة truncation مرتفعة أو أثرًا واضحًا على جودة المهام بسبب طول التسلسل.
* **Rollback path:** إعادة تقييم `max_length` واختيار قيمة أعلى بناءً على قياسات بيانات المشروع، مع الحفاظ على mBERT tokenizer ما لم تظهر مشكلة مرتبطة بالـtokenizer نفسه.

-------

## Decision D-002 — Day 2 Classification, NER & QA

* **Date:** 2026-09-30
* **Gate:** B
* **Status:** accepted
* **Owner:** Student

### Context | السياق

تم تنفيذ مهام التصنيف وNER وExtractive QA باستخدام بيانات صناعية صغيرة، مع مقارنة baseline بسيط بنموذج multilingual Transformer. الهدف هو إثبات صحة الـpipeline والتقييم، وليس إثبات أداء إنتاجي.

### Checkpoint | نقطة الحفظ

تم اعتماد أفضل checkpoint للتصنيف بناءً على **Validation Macro-F1**.

* Best checkpoint: **Epoch 9**
* Validation Macro-F1: **1.0000**
* Baseline Validation Macro-F1: **0.6667**
* Transformer Test Macro-F1: **0.8667**
* Transformer Test Accuracy: **0.875**

تم استخدام validation لاختيار checkpoint، ثم تم تقييم النموذج على test.

### Execution type | نوع التنفيذ

تم استخدام:

**Partial Fine-tuning on CPU**

* معظم طبقات Transformer كانت مجمدة.
* تم تحديث آخر Transformer block وtask head.
* لم يتم استخدام full fine-tuning بسبب قيود التنفيذ على CPU.

### Split strategy & leakage evidence | استراتيجية التقسيم ودليل عدم التسرب

تم تقسيم بيانات التصنيف إلى:

* Train: **24**
* Validation: **8**
* Test: **8**

وكان:

* `group_overlap = 0`
* جميع الفئات الأربع موجودة في train/validation/test.
* **Split contract = PASS**

وبالتالي لا يوجد تداخل للمجموعات بين الـsplits في عينة التصنيف المستخدمة.

### Baseline & Transformer metrics | مقاييس الـBaseline والـTransformer

| Model                   | Validation Macro-F1 | Test Macro-F1 | Test Accuracy |
| ----------------------- | ------------------: | ------------: | ------------: |
| TF-IDF + LinearSVC      |              0.6667 |        0.7333 |             — |
| Multilingual DistilBERT |              1.0000 |        0.8667 |         0.875 |

النتائج مصنفة **MEASURED_SMOKE** وليست benchmark إنتاجيًا.

### NER alignment policy | سياسة محاذاة NER

تم اعتماد المحاذاة التالية:

* أول subword للكلمة يحصل على label الكلمة.
* continuation subwords تحصل على `-100`.
* special tokens تحصل على `-100`.
* يتم التعامل مع حدود الكيانات باستخدام **strict entity boundaries**.

Evidence:

* `NER alignment contract = PASS`
* `Strict entity-boundary test = PASS`

نتيجة NER على عينة الاختبار:

* Precision: **0.6667**
* Recall: **0.5000**
* F1: **0.5714**

### QA null policy | سياسة الإجابة الفارغة في QA

إذا لم توجد إجابة صحيحة داخل الـcontext، يسمح النظام بإرجاع:

`None`

مع السبب:

`no_answer_in_context`

تم اختبار حالتي:

* Valid answer span → **PASS**
* No-answer → **PASS**

كما تم التحقق من تحويل answer offsets إلى token positions.

### What the small sample cannot prove | ما الذي لا تستطيع العينة الصغيرة إثباته

العينة الحالية **لا تستطيع إثبات**:

* جودة النموذج على بيانات حقيقية واسعة النطاق.
* التعميم على جميع أنواع النصوص العربية والإنجليزية.
* استقرار نتائج التصنيف أو NER أو QA على عينات أكبر.
* عدم وجود مشاكل أداء أو تحيزات على بيانات الإنتاج.
* أن نتيجة Validation Macro-F1 = 1.0000 تمثل أداءً إنتاجيًا.
* جودة QA الفعلية؛ نتائج QA الحالية هي **smoke evidence** وليست accuracy benchmark.

### Evidence | الدليل

* **Lab 3 commit:** `b5672665d06a2119c17f167ee0797194e4820128`
* **Lab 4 commit:** `5e3b7e50cff65c8eb54bc1382ad61951997576ea`
* **Classification core:** `DAY2_NOTEBOOK3_CORE=PASS`
* **NER/QA core:** `DAY2_NOTEBOOK4_CORE=PASS`
* **Classification split isolation:** `PASS`
* **NER alignment:** `PASS`
* **QA post-processing:** `PASS`

### Consequences and rollback | الأثر والرجوع

* **Positive consequence:** أصبح لدى المشروع baseline واضح، Transformer checkpoint محدد، split موثق، وسياسات NER وQA قابلة للاختبار.
* **Limitation/new risk:** صغر البيانات يجعل النتائج مناسبة لإثبات صحة الـpipeline فقط، وليس لتقدير الأداء الإنتاجي.
* **Rollback trigger:** ظهور تسرب بيانات أو تدهور واضح عند استخدام بيانات أكبر وأكثر واقعية.
* **Rollback path:** إعادة بناء الـsplit أو تعديل checkpoint/training configuration وإعادة تشغيل التقييم والاختبارات.


------------------------------------------------

---

## قرارات إلزامية قبل Gate E

* [x] tokenizer + max length.
* [ ] Arabic preprocessing profile.
* [ ] task model/baseline and split.
* [ ] semantic encoder/index/k/threshold.
* [ ] metric/slices/error priorities.
* [ ] performance budget.
* [ ] ONNX/INT8 adopt or reject.
* [ ] served artefact + preprocessing/label versions.
