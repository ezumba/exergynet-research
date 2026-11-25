# EDT-BENCH-001 Methodology

**Version:** R5 (2026-11-25)  
**Benchmark ID:** EDT-BENCH-001  
**Contact:** EDT INC / ai@exergynet.org

---

## Evaluation Framework

EDT-BENCH-001 evaluates ExergyNet's xLMP Semantic Router on **LongMemEval-S** (Wu et al., 2024), a standardized benchmark for chat assistant long-term memory across 500 questions drawn from simulated multi-session conversation histories.

---

## Dataset

- **Source:** LongMemEval (Wu et al., 2024), arXiv:2410.10813, ICLR 2025
- **Split:** LongMemEval-S (standard difficulty split)
- **Questions:** 500 unique questions, zero duplicates, zero skips
- **Question types (6):** Single-Session User (SSU, n=70), Single-Session Assistant (SSA, n=56), Knowledge Update (KU, n=78), Multi-Session (MS, n=133), Temporal Reasoning (TR, n=133), Single-Session Preference (SSP, n=30)
- **Dataset integrity:** `longmemeval_s_cleaned.json`, SHA-256 `d6f21ea9d60a0d56f34a05b609c79c88a451d2ae03597821ea3d5a9678c3a442`

---

## Systems Evaluated

### B3 — Session-Level BM25 Baseline

The session-level BM25 baseline (B3) retrieves the top-10 most-relevant full conversation sessions for each question using rank_bm25.

- **Retrieval granularity:** Session-level
- **Retrieval algorithm:** BM25 (rank_bm25)
- **Top-k:** 10 sessions per question
- **Token budget:** Unconstrained — full sessions concatenated into prompt
- **Mean tokens/query (provider-reported):** 29,382
- **Total tokens (provider-reported):** 14,690,941 across 500 questions

### B1 — Oracle Baseline

The Oracle baseline (B1) is the benchmark's theoretical ideal-retrieval reference: it uses the `longmemeval_oracle.json` split, which provides only the evidence-bearing sessions for each question.

- **Retrieval:** Oracle (benchmark-provided evidence sessions only)
- **Mean sessions per question:** ~1.9
- **Mean tokens/query (estimated via local tokenizer):** ~5,600
- **This is not a system ExergyNet operates** — it is the benchmark's ideal ceiling assuming perfect retrieval

### VERIFY-006 — ExergyNet xLMP

ExergyNet's xLMP Semantic Router retrieves at turn level, with per-turn BM25 indexing and a bounded context window.

- **Retrieval granularity:** Turn-level (user and assistant turns indexed separately)
- **Retrieval algorithm:** VMN (Vanguard Memory Node) turn-level BM25
- **Top-k:** 10 turns per question
- **Context window:** Bounded (900 character per indexed turn at ingest; retrieval returns top-10 turn chunks)
- **Mean tokens/query (provider-reported):** 2,456
- **Total tokens (provider-reported):** 1,227,949 across 500 questions
- **Indexed content:** Both user and assistant conversational content

---

## Generation Model

- **Model:** `gemini-3.8-flash`
- **Temperature:** 0
- **Max output tokens:** 16,000
- **Prompting:** Chain-of-thought enabled, same prompt template applied across all systems
- **Evaluation model:** `gemini-3.8-flash` (same model family as generation — not independent third-party evaluation)

---

## Evaluation Protocol

Questions were submitted to each system under identical conditions. For each question, the system retrieved relevant memory chunks and assembled a prompt; the generation model produced an answer; the evaluation model judged correctness against the LongMemEval reference answer.

**Experimental scope:** Single-run evaluation. Temperature=0 reduces but does not eliminate provider-side nondeterminism. Results represent one evaluation run per system.

---

## Statistical Analysis

The McNemar test was applied to the paired 500-question comparison between xLMP and Oracle. The contingency matrix:

|  | Oracle Correct | Oracle Wrong |
|--|----------------|--------------|
| **xLMP Correct** | 314 (n11) | 67 (n10) |
| **xLMP Wrong** | 58 (n01) | 61 (n00) |

- Discordant pairs: b = 67, c = 58
- McNemar statistic: (|b−c|−1)² / (b+c) = (8)² / 125 = 0.512
- Exact two-tailed p-value: 0.474422
- 95% CI for accuracy difference (normal approx): [−2.6 pp, +6.2 pp]

The result is not statistically significant at any conventional threshold.

---

## Known Limitations

### 1. Granularity Attribution (Primary Limitation)

xLMP retrieves at turn level; B3 retrieves at session level. Wu et al. (2024) document that turn-level retrieval substantially outperforms session-level retrieval on LongMemEval. Without an independent turn-level BM25 control (B4), the full +19.8pp accuracy improvement cannot be attributed solely to xLMP's semantic routing — an unknown fraction may reflect the inherent advantage of finer retrieval granularity.

### 2. Single Evaluation Run

Each system was evaluated in a single run. While temperature=0 is used, provider-side nondeterminism cannot be fully eliminated. Results may vary across runs.

### 3. Same-Model Evaluation

The evaluation judge (`gemini-3.8-flash`) is the same model family as the generation model. This introduces potential for systematic bias that independent evaluation would not. Results should be interpreted with this in mind.

### 4. Oracle Token Estimation

Oracle (B1) token counts were estimated via a local tokenizer, not provider-reported. The estimate (~5,600 tokens/query) is directional only. Provider-reported token counts for B3 and VERIFY-006 are exact.

### 5. Restricted Artifacts

Per-question generation and evaluation logs are sealed and restricted. The final report's SHA-256 is independently verifiable. Full artifact access for research purposes: ai@exergynet.org.

---

## References

Wu, D., Wang, H., Yu, W., Zhang, Y., Chang, K.-W., & Yu, D. (2024). LongMemEval: Benchmarking Chat Assistants on Long-Term Interactive Memory. *arXiv:2410.10813*. ICLR 2025. https://github.com/xiaowu0162/LongMemEval
