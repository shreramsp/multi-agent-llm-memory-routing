# Multi-Agent LLM Memory Routing

A breakdown of **Learning What to Retain: Gated-Memory Routing for Efficient Collaboration in Multi-Agent LLM Systems**.

- **Authors:** Rakibul Hasan Rajib, Mengxin Zheng, Qian Lou
- **Venue:** EMNLP 2026 (Main)
- **Paper:** [arXiv:2609.00237](https://arxiv.org/abs/2609.00237)

## 🧠 What the Paper Is About

When several LLM agents work together on a task, something has to decide which agent (and which backbone model) acts next. If that decision looks only at the original query, it can't react to progress or mistakes along the way. If it looks at the full execution history, it carries redundant steps and costs more.

This paper instead routes each step using the query plus a compact, **learned execution memory**:

- **Memory Write Gate**: decides which reasoning steps are worth keeping in memory
- **Retrieval Gate**: gives each agent only the relevant part of that memory
- **Adaptive Halting Controller**: stops execution once the memory holds enough evidence to answer

Across five reasoning and code-generation benchmarks, it gets the best average accuracy (+2.44 points over the strongest baseline) and cuts inference cost on HumanEval by 31.9%.

## 📦 Coming Soon

- [ ] Medium article
- [ ] YouTube video
- [ ] Slides
