# ExergyNet Achieves 76.2% Accuracy on LongMemEval-S with 91.6% Lower Context Consumption

**Benchmark:** LongMemEval-S (500 questions, conversational memory evaluation)  
**Date:** 2026-11-25  
**Classification:** PUBLIC RELEASE — AUTHORIZED 2026-11-25  
**Report version:** R5  
**Contact:** EDT INC / ai@exergynet.org  

---

## The Result

ExergyNet's xLMP Semantic Router achieves **76.2% accuracy** on LongMemEval-S while consuming a mean of **2,456 tokens per query** — a **91.6% reduction** in context consumption versus a standard BM25 session-level RAG baseline (29,382 tokens/query).

| System | Accuracy | Tokens/Query |
|--------|----------|--------------|
| BM25 RAG (B3, baseline) | 56.4% | 29,382 |
| Oracle (B1)† | 74.4% | ~5,600 (est.) |
| **ExergyNet xLMP (VERIFY-006)** | **76.2%** | **2,456** |

†Oracle: `longmemeval_oracle.json` split — only evidence-bearing sessions provided (mean ~1.9 sessions per question; mean context ~5,600 tokens, estimated via local tokenizer). This is the benchmark's theoretical ideal-retrieval baseline, not the full conversation history.

**ExergyNet xLMP delivers:**
- **+19.8 percentage points** above the BM25 RAG baseline (56.4% → 76.2%)
- **1.8 pp higher observed accuracy** than the Oracle baseline (74.4% → 76.2%)
- **91.6% fewer context tokens** per query than BM25 RAG (29,382 → 2,456)

---

## Statistical Note on Oracle Comparison

The 1.8-percentage-point difference between xLMP (76.2%) and the Oracle (74.4%) was **not statistically significant** (McNemar test, n=500 paired questions, exact two-tailed p = 0.474). This result does not establish superiority, non-inferiority, or statistical equivalence between the two systems.

Contingency summary (500 matched questions): both correct = 314; xLMP correct, Oracle wrong = 67; xLMP wrong, Oracle correct = 58; both wrong = 61.

An approximate 95% confidence interval for the accuracy difference (normal approximation; SE = √((b+c)/n²) = √(125/500²)) is [−2.6 pp, +6.2 pp], consistent with the non-significant result.

---

## By Question Type

| Question Type | n | ExergyNet xLMP | Oracle | BM25 Baseline | Δ vs BM25 |
|---------------|---|----------------|--------|---------------|-----------|
| Single-Session User | 70 | 94.3% | 94.3% | 84.3% | +10.0 pp |
| Single-Session Assistant | 56 | 80.4% | 100.0% | 83.9% | −3.5 pp |
| Knowledge Update | 78 | 91.0% | 85.9% | 68.0% | +23.0 pp |
| Multi-Session | 133 | 56.4% | 60.9% | 36.1% | +20.3 pp |
| Temporal Reasoning | 133 | 78.2% | 66.2% | 53.4% | +24.8 pp |
| Single-Session Preference | 30 | 66.7% | 46.7% | 13.3% | +53.3 pp |
| **Overall** | **500** | **76.2%** | **74.4%** | **56.4%** | **+19.8 pp** |

ExergyNet xLMP beats BM25 on 5 of 6 question types. ExergyNet xLMP matches or exceeds Oracle accuracy across four of six question categories (wins: Knowledge Update +5.1pp, Temporal Reasoning +12.0pp, Single-Session Preference +20.0pp; tie: Single-Session User 0.0pp; lower: Single-Session Assistant −19.6pp, Multi-Session −4.5pp).

**Single-Session Assistant note:** ExergyNet achieved higher accuracy than session-level BM25 across five of six question categories. Single-Session Assistant accuracy was 80.4%, compared with 83.9% for BM25 and 100.0% for the Oracle baseline. Internal forensic analysis identified retrieval misses in two of the eleven incorrect responses and long-response truncation in the remaining nine. The evaluated VERIFY-006 configuration indexed both user and assistant conversational content. The Oracle provides complete sessions in which all assistant-turn content is available; the observed 100.0% SSA accuracy is consistent with this architecture, though the causal relationship has not been independently established.

---

## Context Efficiency

| Metric | BM25 (B3) | xLMP (VERIFY-006) | Reduction |
|--------|-----------|-------------------|-----------|
| Mean tokens/query | 29,382 | **2,456** | **91.6%** |
| Total prompt tokens (500q) | 14,690,941 | **1,227,949** | **91.6%** |
| Correct answers delivered | 282 | **381** | +99 answers |

Token counts: both B3 and VERIFY-006 figures are provider-reported (Gemini API). The 91.6% context reduction is computed from these verified provider totals.

---

## Benchmark Methodology

- **Dataset:** LongMemEval-S, 500 questions, 6 question types (single-session user/assistant/preference, knowledge-update, multi-session, temporal-reasoning)
- **Evaluation scope:** Single-run evaluation; temperature=0 reduces but does not eliminate provider-side nondeterminism
- **B3 Retrieval:** rank\_bm25, session-level, top-10 sessions per question
- **xLMP Retrieval:** VMN turn-level BM25, top-10 turns per question; per-turn indexed corpus; bounded context window
- **Generation model:** `gemini-3.8-flash`, temperature=0, max\_tokens=16,000, chain-of-thought enabled
- **Evaluation model:** `gemini-3.8-flash` (matched generation methodology — same model and prompt template, not independent third-party evaluation)
- **Oracle (B1):** `longmemeval_oracle.json` split; evidence-bearing sessions only; no retrieval truncation applied
- **Question completeness:** 500/500 unique LongMemEval-S question IDs generated and evaluated; zero duplicates, zero skips

---

## Known Limitation: Granularity Attribution

xLMP retrieves at turn level rather than session level. Wu et al. (2024) document that turn-level retrieval substantially outperforms session-level retrieval on LongMemEval. An independent turn-level BM25 control experiment (B4) would isolate what fraction of the accuracy improvement is specifically attributable to xLMP's semantic routing versus the inherent advantage of finer retrieval granularity. That experiment has not been conducted. Without B4, the full +19.8pp accuracy improvement cannot be attributed solely to xLMP.

---

## Summary

ExergyNet delivered **35.1% more correct answers** than session-level BM25 (381 vs 282) while consuming **91.6% fewer context tokens** across 500 LongMemEval-S questions.

---

## References

Wu, D., Wang, H., Yu, W., Zhang, Y., Chang, K.-W., & Yu, D. (2024). LongMemEval: Benchmarking Chat Assistants on Long-Term Interactive Memory. *arXiv:2410.10813*. ICLR 2025.

---

## Sealed Artifacts

| Artifact | SHA-256 |
|----------|---------|
| Generation log (`c3_meta_verify006_gen.jsonl`, 500 records) | `cb154d2949b6c6bf94406c11a8f80ec373e3bececa2e066e61fb074b9d0bfa1e` |
| Evaluation log (`c3_meta_verify006_eval.jsonl`, 500 records) | `4ddaf962cdb5b6152bed91da4b4b14f0cb2cea000055b93e57f103a75b3d3719` |
| Retrieval corpus (`c3_meta_v3_vmn_top10_full.jsonl`) | `7cdcf2860d4fa58d3b35aae602598f1a397702cd3aeaf0d30957b10566ebc532` |
| Reference dataset (`longmemeval_s_cleaned.json`) | `d6f21ea9d60a0d56f34a05b609c79c88a451d2ae03597821ea3d5a9678c3a442` |
| Reproducibility manifest | `EDT_BENCH_001_C3_VERIFY_006_REPRODUCIBILITY_MANIFEST.json` |

---

*ExergyNet — Indianapolis*  
*EDT-BENCH-001 | Sealed 2026-11-25*  
*EDT INC / ai@exergynet.org*
