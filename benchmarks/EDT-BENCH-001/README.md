# EDT-BENCH-001: LongMemEval-S Benchmark

**ExergyNet Achieves 76.2% Accuracy on LongMemEval-S with 91.6% Lower Context Consumption**

**Published by:** EDT INC  
**Date:** 2026-11-25  
**Benchmark:** LongMemEval-S (500 questions)  
**Contact:** [ai@exergynet.org](mailto:ai@exergynet.org)

---

## Results at a Glance

| System | Accuracy | Mean Prompt Tokens |
|---|---:|---:|
| Session-level BM25 (B3) | 56.4% | 29,382 |
| Evidence-only Oracle (B1)† | 74.4% | ~5,600 est. |
| **ExergyNet xLMP** | **76.2%** | **2,456** |

†Oracle token count is a local-tokenizer estimate. B3 and xLMP token counts are provider-reported from the Gemini API.

**ExergyNet xLMP delivers:**
- +19.8 percentage points above session-level BM25 (56.4% → 76.2%)
- 35.1% more correct answers (381 vs. 282)
- 91.6% lower provider-reported prompt token consumption (29,382 → 2,456 tokens/query)

---

## Results by Question Type

| Question Type | n | ExergyNet xLMP | Oracle | BM25 | Δ vs BM25 |
|---|---:|---:|---:|---:|---:|
| Single-Session User | 70 | 94.3% | 94.3% | 84.3% | +10.0 pp |
| Single-Session Assistant | 56 | 80.4% | 100.0% | 83.9% | −3.5 pp |
| Knowledge Update | 78 | 91.0% | 85.9% | 68.0% | +23.0 pp |
| Multi-Session | 133 | 56.4% | 60.9% | 36.1% | +20.3 pp |
| Temporal Reasoning | 133 | 78.2% | 66.2% | 53.4% | +24.8 pp |
| Single-Session Preference | 30 | 66.7% | 46.7% | 13.3% | +53.3 pp |
| **Overall** | **500** | **76.2%** | **74.4%** | **56.4%** | **+19.8 pp** |

---

## Statistical Note

The 1.8 pp difference between xLMP (76.2%) and the Oracle (74.4%) was **not statistically significant** (McNemar test, exact two-tailed p = 0.474, n = 500). This result does not establish superiority, non-inferiority, or statistical equivalence. 95% CI for the accuracy difference: [−2.6 pp, +6.2 pp] (normal approximation).

Contingency: both correct = 314; xLMP correct / Oracle wrong = 67; xLMP wrong / Oracle correct = 58; both wrong = 61.

---

## Methodology

- **Dataset:** LongMemEval-S, 500 questions, 6 question types
- **Evaluation scope:** Single-run; temperature=0
- **Comparators:** Session-level BM25 (rank_bm25, top-10 sessions) and evidence-only Oracle split
- **Generation model:** `gemini-3.8-flash`, temperature=0, max_tokens=16,000, chain-of-thought enabled
- **Evaluation model:** `gemini-3.8-flash` (matched methodology — not independent third-party evaluation)
- **Oracle split:** `longmemeval_oracle.json` — evidence-bearing sessions only (~1.9 sessions/question)

## Known Limitations

1. **Granularity attribution:** Without a turn-level BM25 control (B4), the full +19.8 pp improvement cannot be attributed solely to xLMP's semantic routing.
2. **Single-run:** Results reflect one evaluation pass at temperature=0.
3. **Same-model evaluation:** Generation and evaluation used the same model family.
4. **SSA forensic:** Internal analysis identified truncation (9/11) and retrieval misses (2/11) in Single-Session Assistant errors.

---

## Files in This Package

| File | Description |
|---|---|
| `README.md` | This file |
| `EDT_BENCH_001_FINAL_REPORT.md` | Complete technical report (R5) |
| `BENCHMARK_RESULTS.csv` | Machine-readable aggregate results |
| `BENCHMARK_RESULTS.json` | Structured benchmark results with metadata |
| `METHODOLOGY.md` | Detailed methodology documentation |
| `PUBLIC_REPRODUCIBILITY_MANIFEST.json` | Public reproducibility evidence |
| `CITATION.cff` | Citation metadata |
| `SHA256SUMS.txt` | Artifact integrity hashes |
| `figures/overall_accuracy.svg` | Overall accuracy comparison chart |
| `figures/context_consumption.svg` | Prompt token consumption chart |
| `figures/accuracy_by_category.svg` | Accuracy by question type |
| `figures/accuracy_context_comparison.svg` | Accuracy vs. token consumption |

---

## Citation

```
EDT INC. (2026). ExergyNet Achieves 76.2% Accuracy on LongMemEval-S with
91.6% Lower Context Consumption (EDT-BENCH-001). ExergyNet Research.
https://doi.org/10.5281/zenodo.24544305
```

See `CITATION.cff` for structured citation metadata.

---

## References

Wu, D., Wang, H., Yu, W., Zhang, Y., Chang, K.-W., & Yu, D. (2024). LongMemEval: Benchmarking Chat Assistants on Long-Term Interactive Memory. *arXiv:2410.10813*. ICLR 2025.

---

*EDT INC | ExergyNet Research | ai@exergynet.org*
