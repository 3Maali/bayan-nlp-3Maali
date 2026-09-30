# BENCHMARKS — Bayan

## 1. Claim boundary | حدود الادعاء

* Artefact role: `PROJECT_ARTIFACT`
* Result label: `MEASURED`
* Task: Multilingual topic text classification
* Decision date: 2026-09-30
* Author: Maali Alkhaldi

These results use the actual project model checkpoint and a fixed bilingual validation workload. They are not `SYSTEMS_SMOKE` results.

## 2. Performance budget — written before candidates

| Constraint             |        TARGET | Why this matters                             |
| ---------------------- | ------------: | -------------------------------------------- |
| p95 model-only latency |       1000 ms | Target responsive CPU inference              |
| minimum throughput     |   0.1 items/s | Ensure sufficient CPU serving capacity       |
| maximum quality tax    | 0.05 macro-F1 | Limit quality degradation after optimisation |
| target device          |   `colab-cpu` | Reproducible CPU deployment                  |

* Budget provenance: `STUDENT_DEFINED_BEFORE_MEASUREMENT`
* Commit/time proving budget existed before candidate measurement: 2026-09-29

## 3. Reproduction contract

| Field                        | Value                                                                                     |
| ---------------------------- | ----------------------------------------------------------------------------------------- |
| Colab runtime/Python         | Python 3.13.15                                                                            |
| Platform                     | Linux-6.6.122+-x86_64-with-glibc2.39                                                      |
| Device/provider              | CPU / `CPUExecutionProvider`                                                              |
| CPU/GPU details              | CPU runtime; Colab CPU                                                                    |
| Library versions             | torch 2.11.0+cpu; transformers 5.15.1; tokenizers 0.22.2; onnx 1.22.0; onnxruntime 1.29.0 |
| Preprocessing version        | `ar-en-v1`                                                                                |
| Label map                    | `{0: digital_service, 1: health, 2: permit, 3: transport}`                                |
| Model source                 | `/content/bayan_model_v1`                                                                 |
| Model state SHA256           | `ca3b92ae2f08bdf7e24b339a7ca9f10885d0bb5efec05efa493db78a83c6ab74`                        |
| Workload path                | `/content/bayan_validation.csv`                                                           |
| Workload SHA256              | `9b07010d0e607282b66a0a1b1096175fd6015a237577db26bf03366393e077d8`                        |
| Split                        | validation                                                                                |
| Workload size                | 8 examples                                                                                |
| Languages                    | Arabic: 4, English: 4                                                                     |
| Batch size                   | 4                                                                                         |
| Max sequence length          | 96                                                                                        |
| Padding                      | Dynamic padding per batch                                                                 |
| Length p50                   | 11 tokens                                                                                 |
| Length p95                   | 14.65 tokens                                                                              |
| Maximum length               | 15 tokens                                                                                 |
| Would truncate               | 0 examples                                                                                |
| Warm-up                      | 5                                                                                         |
| Measured repetitions         | 30                                                                                        |
| Primary measurement boundary | Model-only                                                                                |
| Secondary measurement        | PyTorch end-to-end tokenisation + model                                                   |
| Memory method                | Process RSS start and observed peak; approximate                                          |

The workload is fixed by example IDs and SHA256. Candidate measurements use the same workload, device and batch size.

## 4. Controlled candidates

| ID | Runtime/precision         | Only intended change | Artefact hash                                                      | Size MiB |
| -- | ------------------------- | -------------------- | ------------------------------------------------------------------ | -------: |
| A  | PyTorch FP32 reference    | Baseline             | `ca3b92ae2f08bdf7e24b339a7ca9f10885d0bb5efec05efa493db78a83c6ab74` |  516.234 |
| B  | ONNX Runtime FP32         | Runtime/export       | `f66c5bca71a61167bc8e7456f1a2ad12a6873d4ff2b8ffba793c1d9ec670e5da` |  516.348 |
| C  | ONNX Runtime dynamic INT8 | Weight quantisation  | `be605ccbb99d988e9a1a6a05b33c695d1c301b15cb3616df96efdc406a26a198` |  129.445 |

The model weights and ONNX artefacts remain outside GitHub.

## 5. Parity

| Comparison |  max abs logits diff |         mean abs diff | prediction agreement | Verdict                             |
| ---------- | -------------------: | --------------------: | -------------------: | ----------------------------------- |
| A vs B     | 2.86102294921875e-06 | 7.706694304943085e-07 |                  1.0 | PASS                                |
| A vs C     |     1.37660551071167 |    0.3309343457221985 |                  0.5 | FAIL for project quality acceptance |

* Tolerance chosen before inspection: `1e-3` maximum absolute logits difference.
* Rationale: small numerical differences from an FP32 ONNX export/runtime are acceptable when logits remain within a predefined numerical tolerance and predictions agree. The FP32 candidate also achieved 1.0 prediction agreement.
* INT8 was not rejected solely because of numerical difference; its task quality was measured directly on the same project workload.

## 6. Performance results

Primary performance boundary: model-only inference.

| ID |  p50 ms |  p95 ms |  p99 ms | items/s | observed peak RSS MiB | speedup vs A |
| -- | ------: | ------: | ------: | ------: | --------------------: | -----------: |
| A  | 244.124 | 394.442 | 402.342 |  27.805 |              5296.285 |        1.00× |
| B  | 224.786 | 340.680 | 347.205 |  30.986 |              5505.301 |        1.16× |
| C  | 136.091 | 208.943 | 211.488 |  50.157 |              5608.211 |        1.89× |

All measurements used 5 warm-up iterations followed by 30 measured repetitions.

For the PyTorch reference, the secondary end-to-end measurement was:

* p50: 237.924 ms
* p95: 252.743 ms
* p99: 255.088 ms
* throughput: 33.632 items/s
* observed peak RSS: 5296.285 MiB

## 7. Quality results

* Primary task metric: macro-F1
* Evaluation file/split: `/content/bayan_validation.csv` / validation
* Workload size: 8 examples
* Quality metric: `macro_f1_validation_full_workload`

| ID | Task quality | Quality tax = A − candidate | Small-sample/CI note                                          |
| -- | -----------: | --------------------------: | ------------------------------------------------------------- |
| A  |       1.0000 |                      0.0000 | Small validation workload: n=8; no CI estimated               |
| B  |       1.0000 |                      0.0000 | Same 8 validation examples; prediction agreement with A = 1.0 |
| C  |       0.4345 |                      0.5655 | Same 8 validation examples; prediction agreement with A = 0.5 |

The quality result is a small-workload measurement and should not be interpreted as production-level generalisation performance.

## 8. Budget verdict and decision

| Candidate | latency OK | throughput OK | quality OK | Overall |
| --------- | ---------- | ------------- | ---------- | ------- |
| B         | TRUE       | TRUE          | TRUE       | TRUE    |
| C         | TRUE       | TRUE          | FALSE      | FALSE   |

* Selected runtime: `onnx-fp32`
* Decision: **ADOPT_ONNX_FP32**
* Evidence-based reason: ONNX FP32 met all three predefined budget constraints, preserved prediction agreement at 1.0, and achieved a model-only p95 reduction from 394.442 ms to 340.680 ms. Dynamic INT8 was faster and smaller, but its macro-F1 fell to 0.4345, producing a quality tax of 0.5655, above the maximum allowed 0.05.
* Known limitation/noise source: The project workload contains only 8 validation examples, so latency and quality estimates are small-sample measurements. RSS is approximate process-level RSS rather than isolated model memory.
* FP32 rollback/reproduction path: Re-export ONNX FP32 from the recorded `MODEL_SOURCE` `/content/bayan_model_v1`. Model weights remain outside GitHub.
* Generated JSON report: `reports/benchmark_results.json`

## 9. Reproduction commands

The reference model and generated ONNX artefacts are intentionally kept outside GitHub.

The project benchmark was executed from Notebook 08 with:

```bash
pip install onnx onnxruntime
```

Then run the benchmark cells in:

```text
notebooks/08_optimization_serving.ipynb
```

Project configuration:

```text
PROJECT_MODE = True
PROJECT_MODEL_SOURCE = /content/bayan_model_v1
PROJECT_TOKENIZER_SOURCE = /content/bayan_model_v1
PROJECT_VALIDATION_CSV = /content/bayan_validation.csv
PROJECT_PREPROCESSING_VERSION = ar-en-v1
```

## 10. Integrity check

* Budget provenance recorded as `STUDENT_DEFINED_BEFORE_MEASUREMENT`.
* Same fixed workload, device and batch size used for candidates.
* Warm-up excluded from reported measurements.
* 30 measured repetitions used.
* p50/p95/p99 and throughput included.
* Memory wording matches the approximate process RSS measurement method.
* Quality tax uses the same 8 validation examples.
* The failed INT8 candidate is explicitly reported and was not hidden.
* Numbers are `MEASURED` project results, not copied Systems Smoke references.
* FP32 numerical parity was checked with a predefined `1e-3` maximum absolute logits tolerance.
* No weights, ONNX artefacts, cache, secrets, or PII are committed to GitHub.

## Extension

The measured extension is dynamic INT8 quantisation.

* Baseline: ONNX Runtime FP32.
* Candidate: ONNX Runtime dynamic INT8.
* Evidence: `reports/benchmark_results.json`.
* Result: INT8 reduced model-only p95 latency from 340.680 ms to 208.943 ms and reduced artefact size from 516.348 MiB to 129.445 MiB, but macro-F1 decreased from 1.0000 to 0.4345.
* Decision: INT8 was rejected for the project serving path because its quality tax of 0.5655 exceeded the 0.05 budget.
* Selected serving runtime: ONNX Runtime FP32.
