# Learning What to Retain: Gated-Memory Routing for Multi-Agent LLMs — Explained

A paper breakdown with a Medium article, a video walkthrough and slides on how a team of LLM agents can decide **who acts next, which model to use, what to read and when to stop**, by keeping a small, learned memory instead of the full history.

## 📄 Paper Information
- **Title:** Learning What to Retain: Gated-Memory Routing for Efficient Collaboration in Multi-Agent LLM Systems
- **Authors:** Rakibul Hasan Rajib, Mengxin Zheng, Qian Lou (University of Central Florida)
- **Venue:** EMNLP 2026 (Main)
- **ArXiv:** [2609.00237](https://arxiv.org/abs/2609.00237)

## 🔗 Links
- **Paper PDF:** [arXiv](https://arxiv.org/pdf/2609.00237)
- **Medium Article:** [Medium](https://medium.com/@shrerampalanisamy/learning-what-to-retain-gated-memory-routing-for-multi-agent-llms-explained-56527f72886e)
- **Video Presentation:** [YouTube](https://youtu.be/E8P5FF0bagE)
- **Slides:** [PDF in this repo](https://github.com/shreramsp/multi-agent-llm-memory-routing/blob/main/SlideShare.pdf)  
  <sub>SlideShare uploads are temporarily unavailable, so the slides are hosted here for now. A SlideShare link will be added once uploads work again.</sub>

## 🎯 Why This Paper?
Multi-agent LLM systems split hard problems across a solver, a critic, a verifier and a debugger, and they beat single models on math and coding. But someone has to route the work. Existing routers fail in opposite directions:

| Approach | What the router sees | Problem |
|---|---|---|
| **Query-only** (e.g. MASRouter) | Only the question | Plans everything up front and can't react to mistakes or progress |
| **Full-history** (e.g. Puppeteer) | Every step so far | Redundant, ever-growing context drives up cost |

This paper asks: **can the router keep good notes instead of the full transcript?**

## 🔑 Key Contributions
1. **Gated execution memory:** every routing decision uses the question plus a compact, learned memory of past steps (role, model, response).
2. **Five learned components** that act on that memory and are trained together.
3. **Joint training with one reward:** correct answer minus cost, optimized with a GRPO-style policy gradient while the LLMs stay frozen.
4. **Better accuracy at lower cost** than both query-only and full-history routing.

## 🧩 How It Works

| Component | Decides | How |
|---|---|---|
| **Role Allocator** | Who acts next | Matches the current state to text descriptions of 26 roles, embedded with a VAE |
| **LLM Router** | Which model powers the step | Picks from Llama-3.2-3B, Llama-3.1-8B, Mistral-Nemo-12B, Qwen-2.5-14B and Qwen-2.5-32B |
| **Retrieval Gate** | What each agent reads | A learned yes/no per memory record, with no fixed top-k |
| **Memory Write Gate** | What gets saved | Learned Maximal Marginal Relevance: relevance minus redundancy |
| **Adaptive Halting Controller** | When to stop | A GRU watches the memory and stops once there is enough evidence (max 6 steps) |

## 📊 Results
Benchmarks: MATH, GSM-Hard, MBPP, HumanEval and MMLU-Pro.

- **Best average accuracy: 77.73%**, +2.44 points over the strongest baseline (Puppeteer with Qwen-2.5-32B)
- **Best on 4 of 5 benchmarks**: MATH 79.33, GSM-Hard 70.55, HumanEval 89.84, MMLU-Pro 69.32
- **HumanEval cost:** 31.9% lower than Puppeteer and 43.86% lower than MASRouter
- **vs. full history:** about the same accuracy, but about 40% cheaper on GSM-Hard and less than half the cost on HumanEval
- **Ablations:** the LLM router drives accuracy (−14 points on GSM-Hard without it); halting drives cost (2.7× higher on HumanEval without it)

## 🚧 Limitations
- Tested only on tasks with verifiable answers
- Cost is estimated from model size, not real latency or memory use
- Evaluated with open models up to 32B parameters

---
*Figures and results belong to the paper's authors (Rajib et al., 2026). This repository is an independent explainer.*
