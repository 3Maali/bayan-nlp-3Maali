# Applied Natural Language Processing

> **مشروع بيان | Bayan Project:** مشروع تعليمي ثنائي اللغة يطبق تقنيات معالجة اللغات الطبيعية بالعربية والإنجليزية من معالجة النصوص والترميز إلى التصنيف، NER، QA، البحث الدلالي، التقييم وتحسين الاستدلال.

**المتدرب | Learner:** Maali · `3Maali`
**المستودع | Repository:** `https://github.com/3Maali/bayan-nlp-3Maali`
**المقرر | Course:** `SDA-AIE-211`
**السياق التدريبي | Training context:** SDAIA Academy
**البيئة | Environment:** Google Colab Free + GitHub
**الإصدار النهائي | Final release:** `submission-v1.0`

---

## عن المشروع | About Bayan

مشروع **بيان** هو مشروع تطبيقي تعليمي ثنائي اللغة للعربية والإنجليزية، يمر عبر مراحل متعددة من دورة بناء أنظمة NLP:

* معالجة النصوص وتوحيدها.
* Tokenization باستخدام نماذج متعددة اللغات.
* تصنيف الموضوع والمشاعر.
* Named Entity Recognition (NER).
* Extractive Question Answering مع دعم no-answer.
* البحث الدلالي ثنائي اللغة باستخدام embeddings وFAISS.
* تقييم الأداء وتحليل الأخطاء.
* تحسين الاستدلال وبناء API باستخدام FastAPI.

البيانات المستخدمة في المشروع **تعليمية اصطناعية/عامة وليست بيانات مستفيدين حقيقية**.

---

## نطاق المشروع | Scope

يشمل المشروع:

* العربية والإنجليزية.
* Text Processing وTokenization.
* Topic Classification وSentiment Classification.
* NER.
* Extractive QA.
* Bilingual Semantic Search.
* Evaluation وError Analysis.
* ONNX Optimization وFastAPI Serving.

### خارج النطاق | Non-goals

* لا يمثل المشروع نظامًا إنتاجيًا.
* لا يستخدم بيانات مستفيدين حقيقية.
* لا يتم استخدام النتائج لاتخاذ قرارات حكومية أو قرارات تخص أفرادًا.
* النتائج الصغيرة المستخدمة في بعض اللابات هي `MEASURED_SMOKE` وليست دليلًا على أداء إنتاجي.

---

## رحلة المشروع | Project Journey

| اليوم | المجال                         | المخرجات                                      |
| ----- | ------------------------------ | --------------------------------------------- |
| Day 1 | Text Processing & Tokenization | preprocessing + tokenizer decision            |
| Day 2 | Classification, NER & QA       | classification model + NER + QA               |
| Day 3 | Arabic NLP & Search            | Arabic profile + semantic search + evaluation |
| Day 4 | Optimization & Serving         | ONNX + INT8 evaluation + FastAPI              |

---

# نتائج اللابات | Lab Results

## Day 1 — Text Processing & Tokenization

تمت مقارنة **Local WordPiece** مع **mBERT tokenizer** على عينة تعليمية صغيرة.

النتائج:

| Tokenizer       | Arabic Fertility | English Fertility | Truncation |
| --------------- | ---------------: | ----------------: | ---------: |
| Local WordPiece |             1.39 |              1.32 |         0% |
| mBERT           |             2.89 |              1.57 |        40% |

القرار النهائي:

* استخدام mBERT tokenizer كـ baseline.
* `max_length = 12`
* إعادة تقييم القرار عند الانتقال إلى بيانات المشروع الفعلية.

تم توثيق القرار في:

* `notebooks/01_text_processing_tokenization.ipynb`
* `DECISIONS.md` — القرار `D-001`
* اختبارات tokenization في `tests/`

**Gate A:** `PASS`

---

## Day 2 — Classification, NER & QA

### Topic Classification

* Transformer: Multilingual DistilBERT
* Validation Macro-F1: **1.0000 — MEASURED_SMOKE**
* Test Macro-F1: **0.8667 — MEASURED_SMOKE**
* Test Accuracy: **0.8750 — MEASURED_SMOKE**

تم استخدام split منفصل للتدريب والتحقق والاختبار مع عدم وجود group overlap.

### Sentiment Class
