# معالجة اللغات الطبيعية التطبيقية

# Applied Natural Language Processing

> **مشروع بيان | Bayan Project:** مشروع تعليمي ثنائي اللغة يطبق تقنيات معالجة اللغات الطبيعية بالعربية والإنجليزية من معالجة النصوص والترميز إلى التصنيف، NER، QA، البحث الدلالي، التقييم وتحسين الاستدلال.

**المتدرب | Learner:** Maali · `3Maali`
**المستودع | Repository:** https://github.com/3Maali/bayan-nlp-3Maali
**المقرر | Course:** `SDA-AIE-211`
**السياق التدريبي | Training context:** SDAIA Academy
**البيئة | Environment:** Google Colab Free + GitHub
**الإصدار النهائي | Final release:** `submission-v1.0`

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

## رحلة المشروع | Project Journey

| اليوم | المجال                         | المخرجات                                      |
| ----- | ------------------------------ | --------------------------------------------- |
| Day 1 | Text Processing & Tokenization | preprocessing + tokenizer decision            |
| Day 2 | Classification, NER & QA       | classification model + NER + QA               |
| Day 3 | Arabic NLP & Search            | Arabic profile + semantic search + evaluation |
| Day 4 | Optimization & Serving         | ONNX + INT8 evaluation + FastAPI              |

## نتائج اللابات | Lab Results

### Day 1 — Text Processing & Tokenization

تمت مقارنة tokenizer محلي مع mBERT على عينة تعليمية صغيرة.

القرار النهائي:

* استخدام mBERT tokenizer كـ baseline.
* `max_length = 12`
* الاحتفاظ بقرار إعادة التقييم عند الانتقال إلى بيانات المشروع الفعلية.

**Gate A:** `PASS`

### Day 2 — Classification, NER & QA

#### Topic Classification

* Transformer: Multilingual DistilBERT
* Validation Macro-F1: **1.0000 — MEASURED_SMOKE**
* Test Macro-F1: **0.8667 — MEASURED_SMOKE**
* Test Accuracy: **0.8750 — MEASURED_SMOKE**

تم استخدام split منفصل للتدريب والتحقق والاختبار مع عدم وجود group overlap.

#### NER

* Entity F1: **0.5714 — MEASURED_SMOKE**
* تم استخدام subword alignment مع وضع `-100` للـ continuation/special tokens.

#### Extractive QA

تم تطبيق سياسة واضحة للحالات التي لا تحتوي على إجابة داخل السياق (`no_answer_in_context`).

**Gate B:** `PASS`

### Day 3 — Arabic NLP

تم إنشاء preprocessing profile منفصل للنص المعروض والنص المستخدم للنموذج:

* `display_text`
* `model_text`
* Search profile: `search/1.0.0`
* إزالة التشكيل والتطويل.
* تطبيع بعض أشكال الألف.
* الحفاظ على التاء المربوطة.
* دعم Arabizi كـ passthrough heuristic وليس classifier.

كما تم اختبار نموذج Gulf Arabic على عينة صغيرة، وكانت النتائج جزءًا من smoke evaluation وليست مقارنة إنتاجية.

### Semantic Search

* Encoder: `paraphrase-multilingual-MiniLM-L12-v2`
* Embedding dimensions: `384`
* Index: FAISS `IndexFlatIP`
* Recall@3: **1.0000 — MEASURED_SMOKE**
* MRR@3: **0.6667 — MEASURED_SMOKE**
* No-answer accuracy: **1.0000**

تمت تجربة reranker:

* MRR قبل reranking: `0.6667`
* MRR بعد reranking: `0.7222`
* Delta: `+0.0556`

### Evaluation & Error Analysis

تم تحليل الأخطاء على validation fixture.

أهم أنواع الأخطاء:

* `dialect_gap`: 3
* `hard_or_ambiguous`: 3
* `class_confusion`: 2

ومن أهم الإجراءات المقترحة:

1. زيادة وتحسين أمثلة اللهجة الخليجية.
2. إضافة أمثلة contrastive للفئات المتشابهة.
3. تحسين التعامل مع الطلبات القصيرة والغامضة.

**Gate C:** `PASS`

## Day 4 — Optimization & Serving

تم قياس النموذج الفعلي للمشروع (`PROJECT_ARTIFACT`) على Colab CPU باستخدام workload ثنائي اللغة من 8 أمثلة.

### Performance Budget

* Maximum p95 latency: **1000 ms**
* Minimum throughput: **0.1 items/s**
* Maximum quality tax: **0.05**
* Target device: **Colab CPU**

### النتائج

| Candidate         |           p95 |        Throughput |    Quality | Quality Tax | القرار   |
| ----------------- | ------------: | ----------------: | ---------: | ----------: | -------- |
| PyTorch FP32      |     394.44 ms |     27.80 items/s |     1.0000 |      0.0000 | Baseline |
| ONNX FP32         | **340.68 ms** | **30.99 items/s** | **1.0000** |  **0.0000** | Adopt    |
| ONNX Dynamic INT8 |     208.94 ms |     50.16 items/s |     0.4345 |      0.5655 | Reject   |

**القرار:** `ADOPT_ONNX_FP32`

حقق ONNX FP32 متطلبات الميزانية مع prediction agreement = **1.0** وquality tax = **0.0**. تمت تجربة Dynamic INT8، لكنه لم يحقق شرط الجودة بسبب quality tax مرتفع، لذلك لم يتم اعتماده.

**Gate D:** `PASS`

## Serving

تم اختبار FastAPI على النموذج الفعلي، وشملت الاختبارات:

* `/health`
* Arabic request
* English request
* Empty input rejection
* Unsupported language rejection
* Arabic canary
* English canary

جميع اختبارات الـ API الأساسية نجحت.

## Architecture

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

## الخصوصية والاستخدام المسؤول | Privacy & Responsible Use

* لا يحتوي المستودع على بيانات مستفيدين حقيقية.
* لا يتم رفع tokens أو secrets.
* لا يتم رفع model weights الكبيرة إلى GitHub.
* النتائج الحالية مبنية على بيانات تعليمية صغيرة.
* اختلاف اللهجات العربية، خصوصًا اللهجة الخليجية، يمثل أحد القيود.
* لا ينبغي استخدام النظام لاتخاذ قرارات حكومية أو إنتاجية دون validation إضافي وبيانات ممثلة ومراجعة بشرية.

## هيكل المستودع | Repository Structure

```text
bayan-nlp-3Maali/
├── data/
├── notebooks/
├── reports/
├── scripts/
├── src/
│   └── bayan/
├── tests/
├── BENCHMARKS.md
├── DECISIONS.md
├── EVALUATION_REPORT.md
├── MODEL_CARD.md
├── PROGRESS.md
├── PROJECT_README.md
├── PROJECT_SUMMARY.json
├── SUBMISSION.yml
└── README.md
```

## الأدلة والتوثيق | Evidence

* `DECISIONS.md` — القرارات الفنية عبر اللابات.
* `EVALUATION_REPORT.md` — التقييم وتحليل الأخطاء.
* `MODEL_CARD.md` — معلومات النموذج والاستخدام والقيود.
* `BENCHMARKS.md` — قياسات الأداء والتحسين.
* `PROGRESS.md` — تقدم المشروع والـ gates.
* `PROJECT_SUMMARY.json` — ملخص التسليم.
* `SUBMISSION.yml` — معلومات الإصدار النهائي.
* `tests/` — اختبارات المشروع.

## Reproduce on Google Colab

1. افتح نسخة الـ notebook من GitHub في Google Colab.
2. اختر **Save a copy in Drive**.
3. شغل اللابات بالترتيب من Day 1 إلى Day 4.
4. قبل التسليم استخدم:
   **Runtime → Restart session and run all**
5. لا تضف secrets أو PII أو model weights إلى المستودع.

## التحقق النهائي | Final Validation

```bash
PYTHONPATH=src python scripts/validate_submission.py . --require-tag
PYTHONPATH=src python scripts/preflight_submission.py . --require-tag
```

الإصدار النهائي:

```text
submission-v1.0
```

## المساهمة | Contribution

المشروع فردي. شملت مساهمتي تنفيذ وتطوير مراحل مشروع Bayan التعليمية، من preprocessing وtokenization إلى classification وNER وQA وsemantic search وevaluation وoptimized serving، بالإضافة إلى توثيق القرارات والنتائج والقيود.

تم استخدام أدوات الذكاء الاصطناعي للمساعدة في شرح المفاهيم، مراجعة الكود، troubleshooting، وتنظيم التوثيق. تم تنفيذ النتائج والاختبارات ومراجعتها من قبلي.

## Training Context | السياق التدريبي

هذا المشروع التعليمي أُنجز ضمن دورة:

**Applied Natural Language Processing with Transformers (SDA-AIE-211)**

في سياق **SDAIA Academy**.

**Trainer | المدربة:** Meaad Al-Marri — ميعاد المري

**Course source:** https://github.com/almiyead-rgb/bayan-applied-nlp-course

هذا النسب لا يعني اعتماد المشروع أو ملكية الأكاديمية للكود أو النماذج أو المكتبات أو البيانات التابعة لأطراف أخرى.

## Final Hand-in | التسليم النهائي

أقر بأنني راجعت متطلبات المشروع وملفات الأدلة ونتائج الاختبارات ومتطلبات الخصوصية قبل التسليم النهائي.

**Final tag:** `submission-v1.0`

## License & Acknowledgements

هذا المشروع تعليمي. تخضع المكتبات والنماذج والبيانات والمصادر الخارجية المستخدمة في المشروع لتراخيصها وشروط استخدامها الخاصة.

لا يدعي المشروع ملكية النماذج أو المكتبات أو البيانات أو العلامات المؤسسية التابعة لأطراف أخرى.
