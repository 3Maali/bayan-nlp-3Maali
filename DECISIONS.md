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
