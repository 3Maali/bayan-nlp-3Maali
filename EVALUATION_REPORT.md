# تقرير تقييم بيان | Bayan Evaluation Report

## 1. نطاق التقرير

* تاريخ التشغيل: `2026-09-30`
* commit SHA: `5b41624bb6d942d1936cd7968a2a790a9a1007c7`
* runtime/device: `Google Colab / CPU`
* data version/hash: `Course fixtures; retrieval dataset SHA256: 7708cbe884a3c268d24ed2cb87ad2f0a8b64b2e6fa6b37a32393b6ae3bd50e5b`
* preprocessing profile/version/backend: `Arabic search/1.0.0 + English NFC/whitespace/1.0.0; camel-tools 1.6.0`
* model/checkpoint IDs:

  * Classification: `Multilingual DistilBERT`
  * Retrieval: `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`
  * Reranker: `cross-encoder/mmarco-mMiniLMv2-L12-H384-v1`
* نوع الأرقام: `MEASURED_SMOKE / COURSE_FIXTURE`

## 2. العقود قبل القياس

| العقد                                             | الدليل                             | الحالة |
| ------------------------------------------------- | ---------------------------------- | ------ |
| لا PII حقيقية                                     | Course fixtures / synthetic data   | PASS   |
| train/validation/test بلا leakage                 | group overlap = 0 + frozen test    | PASS   |
| tokenizer/model متطابقان                          | Day 1 tokenizer decision           | PASS   |
| Arabic profile متطابقة في train/index/query/serve | `search/1.0.0` contract            | PASS   |
| corpus/query embeddings مطبعة L2                  | embedding norms ≈ 1                | PASS   |
| frozen test لم يستخدم في tuning                   | threshold tuned on validation only | PASS   |

## 3. نتائج المهام

| المهمة         | المقياس الرئيس    |           النتيجة | CI/تكرار            | مجموعة القياس        |
| -------------- | ----------------- | ----------------: | ------------------- | -------------------- |
| Classification | Macro-F1          |          `0.8667` | single smoke run    | Test, n=8            |
| NER            | strict entity F1  |          `0.5714` | single smoke run    | Lab 4 fixture        |
| QA             | EM/F1 + no-answer |            `PASS` | boundary tests PASS | Lab 4 fixture        |
| Retrieval      | Recall@3 / MRR@3  | `1.0000 / 0.6667` | single smoke run    | Test answerable, n=6 |

## 4. شرائح التقييم

| المهمة     | الشريحة      |  n |      metric | 95% CI          | التحذير/التفسير                            |
| ---------- | ------------ | -: | ----------: | --------------- | ------------------------------------------ |
| Evaluation | language=ar  | 24 | F1 `0.7583` | `0.5607–0.9265` | validation fixture                         |
| Evaluation | language=en  | 12 | F1 `0.8286` | `0.5000–1.0000` | SMALL_SLICE                                |
| Evaluation | variant=Gulf | 12 | F1 `0.6583` | `0.2941–0.8952` | SMALL_SLICE                                |
| Evaluation | length=long  | 18 | F1 `0.5259` | `0.3918–0.8308` | ضعف واضح نسبيًا في الشريحة؛ validation فقط |

## 5. مقارنة الإصدارات

* Model A: `Prediction version A`
* Model B: `Prediction version B`
* observed difference B−A: `+0.0012 Macro-F1`
* paired 95% CI: `-0.1047 إلى +0.0996`
* القرار المهني: `لا تدعم CI ادعاءً اتجاهيًا؛ الفرق المرصود صغير وغير حاسم إحصائيًا في هذه العينة.`

## 6. Behavioural tests

| النوع                 | passed/total | pass rate | فشل مهم                                |
| --------------------- | -----------: | --------: | -------------------------------------- |
| invariance            |        `N/A` |     `N/A` | لم تُسجل كفئة منفصلة                   |
| directional           |        `N/A` |     `N/A` | لم تُسجل كفئة منفصلة                   |
| minimum functionality |        `3/6` |     `50%` | health/transport وroute classification |

أهم الإخفاقات:

* Gulf health request → predicted transport.
* English health request → predicted transport.
* MSA bus route → predicted digital service.

## 7. تحليل الأخطاء

* المصدر: validation + behavioural failures فقط.
* عدد الأخطاء المقروءة يدويًا: `8`
* رابط worksheet داخل المستودع: `reports/day3_error_taxonomy.csv`

| taxonomy tag        | count | مثال آمن مختصر      | الفرضية                 |
| ------------------- | ----: | ------------------- | ----------------------- |
| `dialect_gap`       |     3 | Gulf health request | نقص أمثلة Gulf          |
| `hard_or_ambiguous` |     3 | طلب قصير/غير واضح   | الحاجة إلى context أكبر |
| `class_confusion`   |     2 | route/app/status    | تشابه بين الفئات        |

## 8. الإصلاحات الثلاثة ذات الأولوية

| الأولوية | الدليل                 | الإجراء                                         | metric/slice المتوقع        | الكلفة        | اختبار عدم الرجوع   |
| -------- | ---------------------- | ----------------------------------------------- | --------------------------- | ------------- | ------------------- |
| 1        | `dialect_gap` ×3       | زيادة أمثلة Gulf لفئات الصحة والنقل             | Gulf F1 + behavioural tests | متوسطة        | إعادة اختبارات Gulf |
| 2        | `class_confusion` ×2   | إضافة contrastive examples بين الفئات المتشابهة | Macro-F1 بدون regression    | متوسطة        | paired comparison   |
| 3        | `hard_or_ambiguous` ×3 | إضافة context/guidelines للطلبات القصيرة        | behavioural agreement       | منخفضة–متوسطة | behavioural suite   |

## 9. ما الذي لا تثبته النتائج؟

النتائج مبنية على عينات صغيرة وبيانات course fixtures، وبعض الشرائح صغيرة جدًا. Retrieval وmodel comparisons هي `MEASURED_SMOKE` وليست benchmark إنتاجيًا. النتائج لا تثبت التعميم على بيانات حقيقية أو استقرار الأداء عبر seeds أو بيئات تشغيل مختلفة.

## 10. خلاصة للإدارة

النسخة الحالية تثبت وجود pipeline قابل للقياس للتصنيف، NER، QA، والبحث الدلالي ثنائي اللغة، مع عقود واضحة للـpreprocessing والتقييم. Retrieval حقق Recall@3 = 1.0 على عينة الاختبار، بينما أظهر تحليل الأخطاء ضعفًا في Gulf وبعض حالات الالتباس وطول النص.

الخطوة التالية هي استخدام بيانات المشروع الفعلية، ثم قياس الأداء تحت benchmark ثابت، ومعالجة مشاكل Gulf وclass confusion قبل الانتقال إلى قياسات الأداء وONNX/INT8 ومرحلة الـserving.
