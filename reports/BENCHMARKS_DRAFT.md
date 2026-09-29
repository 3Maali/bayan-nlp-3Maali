# BENCHMARKS draft — SYSTEMS_SMOKE

- Result label: `SYSTEMS_SMOKE`
- Decision scope: `SYSTEMS_SMOKE_NOT_A_SHIP_DECISION`
- Workload SHA-256: `1d1d1c3bef8a582931f6a1c1803671e8fe194fc36ae59cdc4f4feca9fc4d6785`
- Device/provider: `cpu` / `CPUExecutionProvider`
- Warm-up/repetitions: 5/30
- Memory method: process RSS start and observed peak; approximate
- PyTorch p95: 19.855 ms
- ONNX FP32 p95: 6.803 ms
- ONNX FP32 quality tax: 0.000000
- INT8 available: True
- Selected for service: `onnx-dynamic-int8`

- Adoption decision: `ADOPT_INT8`

> Replace this smoke draft with the complete BENCHMARKS template and full project workload before Gate D.
