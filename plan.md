# LLM Architecture Research Agenda

## Executive Summary

This document outlines a research agenda for discovering human-level LLM architectures. Based on a comprehensive survey of current approaches, we identify three primary research directions with concrete milestones and resource estimates.

***

## 1. Survey: Current State of LLM Architectures (2025-2026)

### 1.1 Transformer Dominance

Transformers remain the dominant architecture but face fundamental limitations:

* **Quadratic complexity**: O(n²) attention scales poorly for long sequences
* **Memory bottleneck**: KV cache grows linearly with sequence length
* **Compute inefficiency**: Attention layers are memory-bound, not compute-bound

### 1.2 Emerging Alternatives

| Architecture                   | Key Innovation                                   | Status (2026)                          |
| ------------------------------ | ------------------------------------------------ | -------------------------------------- |
| **Mamba (SSM)**                | Selective state spaces, linear sequence modeling | Production-ready, 5× inference speedup |
| **RetNet**                     | Retention mechanism, parallelizable RNN          | Hybrid adoption in production          |
| **RWKV**                       | Linear attention + RNN hybrid                    | Niche adoption, strong long-context    |
| **Hybrid (Transformer + SSM)** | Best of both worlds                              | Emerging standard (2025-2026)          |
| **MoE (Sparse)**               | Conditional computation, expert routing          | Widely adopted for scale               |
| **1-bit LLMs**                 | Ternary weights (-1, 0, +1)                      | Matching full-precision performance    |

### 1.3 Recent Efficiency Innovations

* **Sparse Attention** (HISA, 2026): Hierarchical indexing for fine-grained sparse patterns
* **Context Compression** (2026): Semi-dynamic compression ratios for soft context
* **Adaptive Resolution** (ResAdapt, 2026): Multimodal efficiency through dynamic compute

***

## 2. Key Gaps and Opportunities

### 2.1 Fundamental Gaps

| Gap                             | Impact                                               | Research Opportunity                               |
| ------------------------------- | ---------------------------------------------------- | -------------------------------------------------- |
| **Long-context reasoning**      | Transformers lose coherence >100K tokens             | SSM-based architectures with million-token context |
| **Training-inference mismatch** | Models trained batch-wise, deployed autoregressively | Unified recurrent/parallel training                |
| **Compute inefficiency**        | 90%+ of FLOPs wasted on ignored context              | Dynamic sparsity, adaptive computation             |
| **Multi-modal unification**     | Separate architectures for text, vision, audio       | Unified sequence modeling (Mamba shows promise)    |
| **Reasoning depth**             | Shallow chain-of-thought, no true planning           | Architectures with explicit working memory         |

### 2.2 Underexplored Directions

1. **Neural Architecture Search for SSMs**: Automated discovery of state space configurations
2. **Hybrid Dynamic Routing**: Input-conditioned switching between transformer and SSM modes
3. **Hierarchical Memory Systems**: Multi-scale memory (working, episodic, semantic)
4. **Biologically-Inspired Attention**: Sparse, content-addressable memory like hippocampus

***

## 3. Proposed Research Directions

### Direction A: Hybrid Transformer-SSM Architectures

**Hypothesis**: Combining transformer attention (for global reasoning) with SSM efficiency (for long-context) achieves optimal performance/efficiency tradeoff.

**Milestones**:

| Phase | Duration     | Deliverable                                    |
| ----- | ------------ | ---------------------------------------------- |
| A1    | Months 1-3   | Literature review, baseline implementations    |
| A2    | Months 4-6   | Hybrid block design (attention + Mamba layers) |
| A3    | Months 7-9   | 1B parameter model, pretraining on 1T tokens   |
| A4    | Months 10-12 | Benchmark vs. pure transformer/SSM baselines   |

**Resources**: 4 FTE researchers, 512 H100 GPU-months, \~$2M compute budget

**Risk**: Medium - hybrids are promising but optimal mixing ratios unknown

***

### Direction B: Adaptive Computation Architecture

**Hypothesis**: Input-conditioned dynamic computation (sparse MoE + adaptive depth) can reduce inference cost 10× without quality loss.

**Milestones**:

| Phase | Duration     | Deliverable                                      |
| ----- | ------------ | ------------------------------------------------ |
| B1    | Months 1-2   | Survey of MoE routing, adaptive depth methods    |
| B2    | Months 3-5   | Novel routing algorithm (load-balanced, stable)  |
| B3    | Months 6-9   | 7B MoE model with 64 experts, 4 active per token |
| B4    | Months 10-12 | Evaluation on efficiency/quality Pareto frontier |

**Resources**: 3 FTE researchers, 256 H100 GPU-months, \~$1M compute budget

**Risk**: Low-Medium - MoE is proven, novelty in routing stability

***

### Direction C: Next-Generation State Space Models

**Hypothesis**: SSMs can match/exceed transformer quality while maintaining linear complexity, enabling million-token context reasoning.

**Milestones**:

| Phase | Duration     | Deliverable                                    |
| ----- | ------------ | ---------------------------------------------- |
| C1    | Months 1-3   | Reproduce Mamba-3B, establish baselines        |
| C2    | Months 4-6   | Novel SSM variants (higher-order, multi-input) |
| C3    | Months 7-10  | 3B SSM model with improved language modeling   |
| C4    | Months 11-12 | Long-context benchmarks (1M+ tokens)           |

**Resources**: 3 FTE researchers, 384 H100 GPU-months, \~$1.5M compute budget

**Risk**: High - SSMs promising but unproven at largest scales

***

## 4. Timeline and Resource Summary

### 4.1 Phase 1: Foundation (Months 1-3)

* Literature reviews complete
* Baseline implementations running
* Team hiring/training complete

**Budget**: $500K (compute + personnel)

### 4.2 Phase 2: Development (Months 4-9)

* All three directions in parallel
* Mid-phase review at Month 6
* Early results from Direction B (lowest risk)

**Budget**: $2.5M

### 4.3 Phase 3: Validation (Months 10-12)

* Full benchmarking suite
* Paper submissions (NeurIPS, ICLR)
* Go/no-go decisions for Year 2

**Budget**: $1.5M

### 4.4 Total Year 1 Budget

| Category                | Amount    |
| ----------------------- | --------- |
| Compute (H100 cluster)  | $3.5M     |
| Personnel (10 FTE)      | $2.0M     |
| Infrastructure/Overhead | $0.5M     |
| **Total**               | **$6.0M** |

***

## 5. Success Criteria

| Metric                       | Target                        | Stretch Goal                       |
| ---------------------------- | ----------------------------- | ---------------------------------- |
| Language modeling perplexity | Match Llama-3-8B              | Beat Llama-3-70B                   |
| Inference throughput         | 5× transformer baseline       | 10× transformer baseline           |
| Context window               | 256K tokens minimum           | 1M+ tokens                         |
| Training efficiency          | 2× tokens/sec vs. transformer | 5× tokens/sec                      |
| Publications                 | 3 top-tier papers             | 5+ papers, 1 best paper nomination |

***

## 6. Next Steps

1. **Immediate (Week 1-2)**: Finalize hiring plan for 10 research positions
2. **Month 1**: Set up compute infrastructure, establish baseline experiments
3. **Month 2**: Begin Direction B (MoE) - fastest path to results
4. **Month 3**: Full team ramp-up, all directions active

***

*Document created: 2026-03-31*
*Author: Head of Research*
*Status: Draft - awaiting review*