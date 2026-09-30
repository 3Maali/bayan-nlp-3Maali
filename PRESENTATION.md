# PRESENTATION — Bayan | عرض بيان

**GitHub username / معرف المتدرب:** 3Maali

## 1. Problem and user | المشكلة والمستخدم

**Bayan** is an applied Arabic/English NLP project covering text classification, sentiment, NER, extractive QA, and bilingual semantic search.

**User / المستخدم:**
A user who needs to classify Arabic/English text, extract entities, answer questions from context, or retrieve semantically relevant text.

**Input / المدخل:**
Arabic or English text, depending on the task.

**Scope / النطاق:**

* Arabic and English NLP.
* Classification and sentiment.
* Named Entity Recognition.
* Extractive Question Answering.
* Bilingual semantic search.
* Optimized model serving.

**Non-goals / خارج النطاق:**

* Production-scale deployment.
* Using private or sensitive user data.
* Treating the small course fixtures as representative of real-world performance.

---

## 2. Architecture | المعمارية

The project follows this flow:

**Input text → Arabic preprocessing → model/embedding layer → task or retrieval pipeline → validation/evaluation → serving API**

For semantic search:

**Query → `model_text` preprocessing → multilingual embeddings → L2 normalization → FAISS `IndexFlatIP` → top-k results → optional reranking → no-answer decision**

For serving:

**API request → input validation → preprocessing/tokenization → ONNX FP32 model → prediction → API response**

Architecture details are documented in [`README.md`](README.md).

---

## 3. Demonstration | التطبيق

### Arabic example

**Input:**
`أحتاج معلومات عن خدمات النقل في الرياض`

**Output evidence:**
The system accepts Arabic input and returns a valid prediction through the serving contract.

Evidence:

* Arabic API test: **PASS**
* Arabic canary: **PASS**
* `Final_NLP.ipynb`
* `reports/service_smoke.json`

### English example

**Input:**
`I need information about transportation services in Riyadh.`

**Output evidence:**
The system accepts English input and returns a valid prediction.

Evidence:

* English API test: **PASS**
* English canary: **PASS**
* `Final_NLP.ipynb`
* `reports/service_smoke.json`

### No-answer / invalid-input

The serving API rejects invalid requests instead of silently producing a prediction.

* Empty input → **422 PASS**
* Unsupported language → **422 PASS**

For semantic search, the system also supports an explicit no-answer policy using a threshold selected on validation data and frozen before test.

### Saved fallback

Saved benchmark and serving evidence is available in:

* `reports/benchmark_results.json`
* `reports/service_smoke.json`

---

## 4. Measured evidence | الدليل المقاس

### Quality

For the Day 2 classification task:

* Validation Macro-F1: **1.0000**
* Test Macro-F1: **0.8667**
* Test accuracy: **0.8750**
* Split: **24 train / 8 validation / 8 test**
* `group_overlap = 0`

NER:

* F1: **0.5714**

Semantic search:

* Recall@3: **1.0000**
* MRR@3: **0.6667**
* No-answer accuracy: **1.0000**

Reports and notebooks:

* `Final_NLP.ipynb`
* `reports/retrieval_metrics.json`
* `reports/day3_evaluation_fixture.json`

### Performance

Final project benchmark:

| Metric                 |  PyTorch FP32 |         ONNX FP32 |
| ---------------------- | ------------: | ----------------: |
| Model-only p95 latency |     394.44 ms |     **340.68 ms** |
| Throughput             | 27.80 items/s | **30.99 items/s** |
| Prediction agreement   |             — |        **1.0000** |
| Quality tax            |             — |        **0.0000** |

Environment:

* CPU
* PyTorch `2.11.0+cpu`
* ONNX `1.22.0`
* ONNX Runtime `1.29.0`
* Python `3.13.15`
* Target: Colab CPU

Evidence:

* `Final_NLP.ipynb`
* `BENCHMARKS.md`
* `reports/benchmark_results.json`

### Measurement label and limits

The final optimization benchmark is labeled **`PROJECT_ARTIFACT`**.

Performance budget:

* Maximum p95 latency: **1000 ms**
* Minimum throughput: **0.1 items/s**
* Maximum quality tax: **0.05**

ONNX FP32 met all three limits.

Dynamic INT8 was evaluated but did not meet the quality limit:

* Quality tax: **0.5655**
* Required maximum: **0.05**

Therefore, INT8 was not adopted for the final serving path.

---

## 5. Decision and ownership | القرار والمساهمة

### My change / measured extension

My main measured extension was the **optimized serving evaluation** connecting the trained Bayan model to ONNX-based inference.

Files:

* `Final_NLP.ipynb`
* `notebooks/08_optimization_serving.ipynb`
* `BENCHMARKS.md`
* `DECISIONS.md`
* `reports/benchmark_results.json`
* `reports/service_smoke.json`

Final project commit:

[8af5508de0ee4ebfdb3df9be3f64006713ce6ba4](https://github.com/3Maali/bayan-nlp-3Maali/commit/8af5508de0ee4ebfdb3df9be3f64006713ce6ba4)

### Baseline, benefit/cost and limitation

**Baseline:** PyTorch FP32 inference.

**Observed ONNX FP32 result:**

* Lower p95 model-only latency: **394.44 → 340.68 ms**
* Higher throughput: **27.80 → 30.99 items/s**
* Prediction agreement: **1.0000**
* Quality tax: **0.0000**

**Cost / limitation:**
The benchmark was conducted on a Colab CPU workload and does not establish production-scale performance.

Dynamic INT8 provided better latency/throughput in the measured run but exceeded the project's quality-tax budget, so it was not selected.

### One code decision I can explain

I can explain why **ONNX FP32 was adopted instead of dynamic INT8**.

The decision was not based only on speed. ONNX FP32 met the predefined latency, throughput, and quality requirements while maintaining full prediction agreement with the PyTorch reference. Dynamic INT8 failed the quality-tax requirement, so it was rejected despite its performance improvement.

---

## Training context | سياق التدريب

**Course:** Applied Natural Language Processing with Transformers (`SDA-AIE-211`)
**Academy:** SDAIA Academy
**Trainer:** Meaad Al-Marri

Course repository:
https://github.com/almiyead-rgb/bayan-applied-nlp-course

**#SDAIAAcademy**

This project was developed as part of the Bayan Applied NLP course. The documented results are course/project measurements and should not be interpreted as production performance.

---

## Verification checklist | قائمة التحقق

* [x] Arabic demonstration
* [x] English demonstration
* [x] Invalid-input behavior
* [x] Quality metrics
* [x] Performance benchmark
* [x] Project artifact measurement
* [x] Optimization decision
* [x] Serving evidence
* [x] Specific contribution and files
* [x] Final project commit documented
