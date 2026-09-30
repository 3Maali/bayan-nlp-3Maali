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

| **اليوم** | **المجال**                     | **المخرجات**                                  |
| --------- | ------------------------------ | --------------------------------------------- |
| Day 1     | Text Processing & Tokenization | preprocessing + tokenizer decision            |
| Day 2     | Classification, NER & QA       | classification model + NER + QA               |
| Day 3     | Arabic NLP & Search            | Arabic profile + semantic search + evaluation |
| Day 4     | Optimization & Serving         | ONNX + INT8 evaluation + FastAPI              |

---

# إعادة التشغيل على Google Colab | Reproduce on Google Colab Free

روابط دفاتر المشروع الرسمية:

| **#** | **Notebook**                   | **Colab**                                                                                                                                   | **Purpose** |
| ----- | ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
| 00    | Runtime Doctor                 | [Open in Colab](https://colab.research.google.com/github/3Maali/bayan-nlp-3Maali/blob/main/notebooks/00_runtime_doctor.ipynb)               | Environment |
| 01    | Text Processing & Tokenization | [Open in Colab](https://colab.research.google.com/github/3Maali/bayan-nlp-3Maali/blob/main/notebooks/01_text_processing_tokenization.ipynb) | Gate A      |
| 02    | Attention & Transformers       | [Open in Colab](https://colab.research.google.com/github/3Maali/bayan-nlp-3Maali/blob/main/notebooks/02_attention_transformers.ipynb)       | T2          |
| 03    | Text Classification            | [Open in Colab](https://colab.research.google.com/github/3Maali/bayan-nlp-3Maali/blob/main/notebooks/03_text_classification.ipynb)          | Gate B      |
| 04    | NER & QA                       | [Open in Colab](https://colab.research.google.com/github/3Maali/bayan-nlp-3Maali/blob/main/notebooks/04_ner_and_qa.ipynb)                   | Gate B      |
| 05    | Arabic NLP                     | [Open in Colab](https://colab.research.google.com/github/3Maali/bayan-nlp-3Maali/blob/main/notebooks/05_arabic_nlp.ipynb)                   | Gate C      |
| 06    | Semantic Search                | [Open in Colab](https://colab.research.google.com/github/3Maali/bayan-nlp-3Maali/blob/main/notebooks/06_semantic_search.ipynb)              | Gate C      |
| 07    | Evaluation & Error Analysis    | [Open in Colab](https://colab.research.google.com/github/3Maali/bayan-nlp-3Maali/blob/main/notebooks/07_evaluation_error_analysis.ipynb)    | Gate C      |
| 08    | Optimization & Serving         | [Open in Colab](https://colab.research.google.com/github/3Maali/bayan-nlp-3Maali/blob/main/notebooks/08_optimization_serving.ipynb)         | Gate D      |

### Final notebook

يوجد أيضًا `Final_NLP.ipynb` في جذر المستودع، وهو الإصدار المدمج الذي يجمع مراحل المشروع النهائية في دفتر واحد.

### Clean-run instructions

1. افتح `00_runtime_doctor.ipynb` أو أحد دفاتر المشروع من GitHub في Google Colab.
2. اختر **Save a copy in Drive**.
3. شغّل الدفاتر بالترتيب من Day 1 إلى Day 4.
4. استخدم **Runtime → Restart session and run all** قبل التسليم النهائي.
5. لا تضع tokens أو PII أو model weights أو روابط Drive خاصة داخل المستودع.

---

# نتائج اللابات | Lab Results

## Day 1 — Text Processing & Tokenization

تمت مقارنة **Local WordPiece** مع **mBERT tokenizer** على عينة تعليمية صغيرة.

| **Tokenizer**   | **Arabic Fertility** | **English Fertility** | **Truncation** |
| --------------- | -------------------: | --------------------: | -------------: |
| Local WordPiece |                 1.39 |                  1.32 |             0% |
| mBERT           |                 2.89 |                  1.57 |            40% |

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

### Sentiment Classification

تم تنفيذ sentiment كـ classification head مستقل عن topic classification.

### NER

* Entity F1: **0.5714 — MEASURED_SMOKE**
* تم استخدام subword alignment.
* continuation/special tokens تستخدم `-100`.

### Extractive QA

تم تطبيق سياسة واضحة للحالات التي لا تحتوي على إجابة داخل السياق:

`no_answer_in_context`

وتم اختبار valid spans وno-answer cases.

**Gate B:** `PASS`

---

# Day 3 — Arabic NLP

تم إنشاء preprocessing profile يفصل بين النص الأصلي والنص المستخدم للنموذج:

* `display_text`
* `model_text`
* Search profile: `search/1.0.0`

وتشمل المعالجة:

* إزالة التشكيل.
* إزالة التطويل.
* تطبيع بعض أشكال الألف.
* الحفاظ على التاء المربوطة.
* دعم Arabizi كـ passthrough heuristic وليس classifier.

تم استخدام `camel-tools==1.6.0`.

تم توثيق القرارات في:

* `notebooks/05_arabic_nlp.ipynb`
* `DECISIONS.md` — القرار `D-003`
* `reports/bayan_arabic_profile.json`

---

# Semantic Search

تم بناء بحث دلالي ثنائي اللغة باستخدام:

* Encoder: `paraphrase-multilingual-MiniLM-L12-v2`
* Embedding dimensions: `384`
* L2 normalization
* FAISS: `IndexFlatIP`

النتائج:

* Recall@3: **1.0000 — MEASURED_SMOKE**
* MRR@3: **0.6667 — MEASURED_SMOKE**
* No-answer accuracy: **1.0000**

تمت تجربة reranker:

* MRR قبل reranking: `0.6667`
* MRR بعد reranking: `0.7222`
* Delta: `+0.0556`

تم توثيق التنفيذ والقياسات في:

* `notebooks/06_semantic_search.ipynb`
* `reports/search_manifest.json`
* `reports/retrieval_metrics.json`
* `DECISIONS.md` — القرار `D-004`

---

# Evaluation & Error Analysis

تم تحليل الأخطاء على validation fixture.

أنواع الأخطاء:

* `dialect_gap`: 3
* `hard_or_ambiguous`: 3
* `class_confusion`: 2

الإجراءات المقترحة:

1. زيادة وتحسين أمثلة اللهجة الخليجية.
2. إضافة أمثلة contrastive للفئات المتشابهة.
3. تحسين التعامل مع الطلبات القصيرة والغامضة.

تم توثيق التحليل في:

* `notebooks/07_evaluation_error_analysis.ipynb`
* `EVALUATION_REPORT.md`
* `reports/day3_slice_report.csv`
* `reports/day3_error_taxonomy.csv`

**Gate C:** `PASS`

---

# Day 4 — Optimization & Serving

تم قياس نموذج المشروع الفعلي باعتباره:

`PROJECT_ARTIFACT`

على Colab CPU باستخدام workload ثنائي اللغة من 8 أمثلة.

## Performance Budget

| **المعيار**         |    **الحد** |
| ------------------- | ----------: |
| Maximum p95 latency |     1000 ms |
| Minimum throughput  | 0.1 items/s |
| Maximum quality tax |        0.05 |
| Target device       |   Colab CPU |

## النتائج

| **Candidate**     |       **p95** |    **Throughput** | **Quality** | **Quality Tax** | **القرار** |
| ----------------- | ------------: | ----------------: | ----------: | --------------: | ---------- |
| PyTorch FP32      |     394.44 ms |     27.80 items/s |      1.0000 |          0.0000 | Baseline   |
| ONNX FP32         | **340.68 ms** | **30.99 items/s** |  **1.0000** |      **0.0000** | Adopt      |
| ONNX Dynamic INT8 |     208.94 ms |     50.16 items/s |      0.4345 |          0.5655 | Reject     |

**القرار:** `ADOPT_ONNX_FP32`

حقق ONNX FP32 متطلبات الميزانية مع:

* Prediction agreement = `1.0`
* Quality tax = `0.0`

تمت تجربة Dynamic INT8، لكنه لم يحقق شرط الجودة بسبب `quality tax = 0.5655`، لذلك لم يتم اعتماده.

تم توثيق القياسات في:

* `notebooks/08_optimization_serving.ipynb`
* `BENCHMARKS.md`

**Gate D:** `PASS`

---

# Serving

تم اختبار FastAPI على نموذج المشروع الفعلي.

الاختبارات شملت:

* `/health`
* Arabic request
* English request
* Empty input rejection
* Unsupported language rejection
* Arabic canary
* English canary

جميع اختبارات API الأساسية نجحت.

---

# Architecture

## Encoder data flow | مسار المدخل عبر المشفّر

يمر النص العربي أو الإنجليزي أولًا عبر المعالجة والترميز، ثم عبر طبقات الـTransformer Encoder قبل الوصول إلى الرأس الخاص بالمهمة أو إلى تمثيل embedding.

```text
Arabic / English Text
        │
        ▼
     Tokenizer
        │
        ├── Input IDs
        └── Attention Mask
                │
                ▼
       Token / Position Embeddings
                │
                ▼
        Transformer Encoder
        ┌───────┴────────┐
        │                │
   Self-Attention    Feed-Forward
        │                │
        └───────┬────────┘
                ▼
      Contextual Hidden States
                │
        ┌───────┼──────────────┐
        ▼       ▼              ▼
   Task Head   Token       Text Embedding
 Classification Representations   │
        │       │              ▼
        │       │             FAISS
        │       │              │
        │       ▼              ▼
        │      NER         Reranking
        │
        ▼
 Topic / Sentiment
```

في بداية المسار، يحول الـTokenizer النص إلى `input_ids`، بينما يحدد `attention_mask` الـtokens الحقيقية ومواقع الـpadding. تدخل هذه التمثيلات إلى طبقات الـTransformer Encoder، حيث تستخدم Self-Attention لبناء تمثيلات سياقية، ثم تمر عبر Feed-Forward وعمليات Residual/Layer Normalization.

بعد طبقات الـEncoder ينتج النموذج **Contextual Hidden States**. يختلف الجزء المستخدم بعد ذلك حسب المهمة:

* **Classification:** تمرير التمثيل المناسب إلى Classification Head لإنتاج فئة الموضوع أو المشاعر.
* **NER:** استخدام تمثيل كل Token لإنتاج تصنيف الكيانات.
* **Extractive QA:** استخدام تمثيلات الـTokens لتحديد موضع بداية ونهاية الإجابة، مع دعم `no-answer`.
* **Semantic Search:** تحويل النص إلى Text Embedding ثم استخدام FAISS للاسترجاع، مع إمكانية تطبيق Reranking.

## Project pipeline

```text
                    Arabic / English Text
                            │
                            ▼
                  Privacy + Preprocessing
                    │              │
                    ▼              ▼
              display_text     model_text
                                   │
             ┌─────────────────────┼─────────────────────┐
             ▼                     ▼                     ▼
       Classification             NER              Semantic Search
       ├─ Topic                   │                 ├─ Embeddings
       └─ Sentiment               │                 ├─ FAISS
                                  │                 └─ Reranking
             │                    │
             └─────────────┬──────┘
                           ▼
                  Extractive QA
                   + no-answer
                           │
                           ▼
                 Evaluation & Errors
                           │
                           ▼
                 ONNX / FastAPI Serving
```

## Attention mask

يمنع `attention_mask` مواقع الـpadding من المشاركة في حساب Attention.

تم إجراء فحص خاص في:

`notebooks/02_attention_transformers.ipynb`

باستخدام جمل عربية بأطوال مختلفة. أظهر الفحص أن أوزان Attention المتجهة إلى مواقع الـpadding كانت `0.0`:

```text
ARABIC_PADDING_MASK_CHECK=PASS
```

## حدود تفسير Attention

أوزان Attention تصف كيفية توزيع الوزن داخل طبقة Attention أثناء الحساب، لكنها **لا تثبت وحدها سبب قرار النموذج ولا تمثل تفسيرًا سببيًا**.

لذلك لا تُستخدم خريطة Attention واحدة كدليل نهائي على سبب تصنيف أو تنبؤ النموذج. يتطلب الادعاء التفسيري اختبارات إضافية، مثل الإزالة أو التبديل أو أساليب Attribution مناسبة.

---

# النتائج الرئيسية | Results

| **Component**        | **Metric**           |                   **Result** | **Evidence**         |
| -------------------- | -------------------- | ---------------------------: | -------------------- |
| Topic classification | Test Macro-F1        |  **0.8667 — MEASURED_SMOKE** | `DECISIONS.md` D-002 |
| Topic classification | Test Accuracy        |  **0.8750 — MEASURED_SMOKE** | `DECISIONS.md` D-002 |
| NER                  | Entity F1            |  **0.5714 — MEASURED_SMOKE** | `DECISIONS.md` D-002 |
| Semantic Search      | Recall@3             |  **1.0000 — MEASURED_SMOKE** | `DECISIONS.md` D-004 |
| Semantic Search      | MRR@3                |  **0.6667 — MEASURED_SMOKE** | `DECISIONS.md` D-004 |
| Serving              | ONNX FP32 p95        |     **340.68 ms — MEASURED** | `BENCHMARKS.md`      |
| Serving              | ONNX FP32 throughput | **30.99 items/s — MEASURED** | `BENCHMARKS.md`      |
| Serving              | Quality tax          |        **0.0000 — MEASURED** | `BENCHMARKS.md`      |

---

# Measured Extension | الامتداد المقاس

* **Extension:** ONNX FP32 optimized serving with dynamic INT8 evaluation.
* **Baseline:** PyTorch FP32.
* **ONNX FP32:** p95 = `340.68 ms`, throughput = `30.99 items/s`.
* **Dynamic INT8:** p95 = `208.94 ms`, throughput = `50.16 items/s`.
* **INT8 quality tax:** `0.5655`.
* **Decision:** `ADOPT_ONNX_FP32`.
* **Evidence:** `BENCHMARKS.md` و`notebooks/08_optimization_serving.ipynb`.

---

# الخصوصية والاستخدام المسؤول | Privacy & Responsible Use

* لا يحتوي المستودع على بيانات مستفيدين حقيقية.
* لا يتم رفع tokens أو secrets.
* لا يتم رفع model weights الكبيرة إلى GitHub.
* النتائج الحالية مبنية على بيانات تعليمية صغيرة.
* اختلاف اللهجات العربية، خصوصًا اللهجة الخليجية، يمثل أحد القيود.
* لا ينبغي استخدام النظام لاتخاذ قرارات حكومية أو إنتاجية دون validation إضافي وبيانات ممثلة ومراجعة بشرية.

---

# المساهمة | Contribution

المشروع فردي، وشملت مساهمتي تنفيذ وتطوير مراحل المشروع وتوثيق القرارات والنتائج والاختبارات.

### تغييرات محددة

**1. قرار mBERT Tokenizer**

في:

`notebooks/01_text_processing_tokenization.ipynb`

قارنت بين Local WordPiece وmBERT tokenizer باستخدام قياسات fertility وtruncation.

بناءً على المقارنة تم اعتماد **mBERT tokenizer كـ baseline** مع `max_length=12`.

الدليل:

* `DECISIONS.md` → `D-001`
* `tests/` → tokenization tests

**2. Arabic Preprocessing**

في:

`notebooks/05_arabic_nlp.ipynb`

نفذت فصل `display_text` عن `model_text` وطبقت search profile `search/1.0.0`.

الدليل:

* `DECISIONS.md` → `D-003`
* `reports/bayan_arabic_profile.json`

**3. Semantic Search**

في:

`notebooks/06_semantic_search.ipynb`

نفذت multilingual embeddings وFAISS وreranking، وقست Recall@3 وMRR@3.

الدليل:

* `reports/retrieval_metrics.json`
* `DECISIONS.md` → `D-004`

**4. Evaluation & Error Analysis**

في:

`notebooks/07_evaluation_error_analysis.ipynb`

نفذت slice evaluation وerror taxonomy.

الدليل:

* `EVALUATION_REPORT.md`
* `reports/day3_error_taxonomy.csv`

**5. Optimization & Serving**

في:

`notebooks/08_optimization_serving.ipynb`

نفذت benchmark لنموذج المشروع الفعلي، وقارنت PyTorch FP32 وONNX FP32 وDynamic INT8.

الدليل:

`BENCHMARKS.md`

**6. Attention & Transformer Verification**

في:

`notebooks/02_attention_transformers.ipynb`

نفذت Scaled Dot-Product Attention، وفحصت دلالة الأقنعة، وتتبعت الـforward pass الفعلي لنموذج متعدد اللغات. كما أضفت فحصًا خاصًا لـArabic Padding Mask للتحقق من عدم حصول مواقع الـpadding على Attention.

الدليل:

* `ARABIC_PADDING_MASK_CHECK=PASS`
* `DAY1_NOTEBOOK2_CORE=PASS`
* شرح مسار الـEncoder وحدود تفسير Attention داخل Notebook 02 وREADME.

---

# AI Assistance

استخدمت **ChatGPT من OpenAI** أثناء تنفيذ المشروع كمساعد في:

* شرح مفاهيم NLP وTransformers وtokenization.
* مراجعة بعض أجزاء الكود.
* troubleshooting لبعض المشكلات البرمجية.
* تنظيم وتنسيق التوثيق وREADME.
* مراجعة صياغة بعض ملفات التسليم.

### طريقة التحقق

لم أستخدم ChatGPT كمصدر للنتائج التجريبية. تم تنفيذ الكود والقياسات والاختبارات في بيئة المشروع، وتمت مراجعة النتائج والمخرجات قبل توثيقها.

أدلة التحقق تشمل:

* `notebooks/`
* `DECISIONS.md`
* `BENCHMARKS.md`
* `EVALUATION_REPORT.md`
* `tests/`
* `reports/`

---

# Training Context | السياق التدريبي

This educational project was developed during:

**Applied Natural Language Processing with Transformers (SDA-AIE-211)**

في سياق **SDAIA Academy**.

**Academy | الأكاديمية:** https://github.com/SDAIAAcademy

**Trainer | المدربة:** Meaad Al-Marri — ميعاد المري

**Course source:** https://github.com/almiyead-rgb/bayan-applied-nlp-course

**#SDAIAAcademy**

هذا النسب لا يعني اعتماد المشروع أو ملكية الأكاديمية للكود أو النماذج أو المكتبات أو البيانات التابعة لأطراف أخرى.

---

# الأدلة والتوثيق | Evidence

* `STUDENT_PROFILE.md`
* `PROGRESS.md`
* `DECISIONS.md`
* `EVALUATION_REPORT.md`
* `BENCHMARKS.md`
* `MODEL_CARD.md`
* `DATA_CARD.md`
* `PROJECT_SUMMARY.json`
* `SUBMISSION.yml`
* `reports/`
* `tests/`

---

# Final Validation

```bash
PYTHONPATH=src python scripts/validate_submission.py . --require-tag
PYTHONPATH=src python scripts/preflight_submission.py . --require-tag
```

الإصدار النهائي:

```text
submission-v1.0
```

---

# Final Hand-in | التسليم النهائي

أقر بأنني راجعت متطلبات المشروع وملفات الأدلة ونتائج الاختبارات ومتطلبات الخصوصية قبل التسليم النهائي.

**Final tag:** `submission-v1.0`

---

# License & Acknowledgements

هذا المشروع تعليمي. تخضع المكتبات والنماذج والبيانات والمصادر الخارجية المستخدمة في المشروع لتراخيصها وشروط استخدامها الخاصة.

لا يدعي المشروع ملكية النماذج أو المكتبات أو البيانات أو العلامات المؤسسية التابعة لأطراف أخرى.
