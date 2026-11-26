# ExergyNet Research

Official ExergyNet research publications, computational efficiency evaluations, benchmark results and approved reproducibility materials. Published by EDT INC.

---

## Featured Benchmark: EDT-BENCH-001

### ExergyNet Achieves 76.2% Accuracy on LongMemEval-S with 91.6% Lower Context Consumption

Across 500 standardized conversational memory questions, ExergyNet's xLMP Semantic Router delivered 381 correct answers while consuming 91.6% fewer provider-reported prompt tokens than the evaluated session-level BM25 baseline.

| Evaluation Condition | Accuracy | Mean Prompt Tokens |
|---|---:|---:|
| Session-level BM25 (B3) | 56.4% | 29,382 |
| Evidence-only Oracle (B1)† | 74.4% | ~5,600 est. |
| **ExergyNet xLMP (VERIFY-006)** | **76.2%** | **2,456** |

†Oracle token count estimated via local tokenizer (not provider-reported). B3 and xLMP token counts are provider-reported from the Gemini API.

### Principal Measured Results

- **500** benchmark questions evaluated (LongMemEval-S, full set)
- **381** correct answers delivered by ExergyNet xLMP
- **+19.8 percentage points** accuracy improvement over session-level BM25
- **35.1% more correct answers** (381 vs. 282)
- **91.6% lower** provider-reported prompt token consumption
- Mean context per query: **2,456 tokens** (vs. 29,382 for session-level BM25)

### Statistical Interpretation

The observed 1.8 percentage-point difference between xLMP (76.2%) and the Oracle (74.4%) was **not statistically significant** (McNemar test, exact two-tailed p = 0.474, n = 500 paired questions). This result does not establish statistical superiority, non-inferiority, or equivalence between the two systems. The 95% confidence interval for the accuracy difference is [−2.6 pp, +6.2 pp] (normal approximation).

### Research Limitations

**Granularity attribution:** xLMP retrieves at turn level while B3 retrieves at session level. Without an independent turn-level BM25 control (B4), the full +19.8 pp accuracy improvement cannot be attributed solely to xLMP's semantic routing versus the inherent advantage of turn-level retrieval granularity.

**Single-run evaluation:** Results reflect a single benchmark run at temperature=0.

**Same-model evaluation:** Generation and evaluation both used `gemini-3.8-flash`. Results have not been independently verified with a third-party evaluation model.

**SSA forensic:** Single-Session Assistant accuracy was 80.4%, compared to 83.9% for BM25 and 100.0% for the Oracle. Internal forensic analysis identified retrieval misses in two of the eleven incorrect responses and long-response truncation in the remaining nine.

---

## Publications

| ID | Title | Type | Date | DOI |
|---|---|---|---|---|
| EDT-BENCH-001 | ExergyNet Achieves 76.2% Accuracy on LongMemEval-S | Benchmark Report | 2026-11-25 | [10.5281/zenodo.24544305](https://doi.org/10.5281/zenodo.24544305) |
| ExergyNet V5 | Memory Grows. Context Does Not Have To. | Research Paper | 2026-11-06 | [10.5281/zenodo.24021817](https://doi.org/10.5281/zenodo.24021817) |

---

## Repository Structure

```
exergynet-research/
├── README.md                           This file
├── CITATION.cff                        How to cite this repository
├── LICENSE-STATUS.md                   Publication license terms
├── publication-index.json              Machine-readable publication index
├── publications/
│   └── exergynet-v5/                   ExergyNet V5 Whitepaper reference
└── benchmarks/
    └── EDT-BENCH-001/                  LongMemEval-S benchmark
        ├── README.md                   Benchmark overview
        ├── EDT_BENCH_001_FINAL_REPORT.md  Full technical report
        ├── BENCHMARK_RESULTS.csv       Machine-readable results
        ├── BENCHMARK_RESULTS.json      Structured results data
        ├── METHODOLOGY.md              Methodology documentation
        ├── PUBLIC_REPRODUCIBILITY_MANIFEST.json
        ├── CITATION.cff                How to cite EDT-BENCH-001
        ├── SHA256SUMS.txt              Artifact integrity
        └── figures/                   Research visualizations
```

---

## Benchmark Dataset

This benchmark evaluates ExergyNet against [LongMemEval](https://github.com/xiaowu0162/LongMemEval) (Wu et al., 2024), a standardized benchmark for assessing conversational memory in AI systems.

Wu, D., Wang, H., Yu, W., Zhang, Y., Chang, K.-W., & Yu, D. (2024). LongMemEval: Benchmarking Chat Assistants on Long-Term Interactive Memory. *arXiv:2410.10813*. ICLR 2025.

---

## Contact

**EDT INC** | ExergyNet Research  
[ai@exergynet.org](mailto:ai@exergynet.org)  
[https://exergynet.org](https://exergynet.org)

---

*Research publications by EDT INC. ExergyNet technology and architecture are proprietary. Publication of benchmark results does not constitute an open-source license for ExergyNet components.*
