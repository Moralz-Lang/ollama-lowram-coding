# Low‑Resource LLM Reasoning & Preference Optimization

> A technical research and engineering guide for deploying, controlling, and evaluating large language models under extreme hardware constraints.

---

## Overview

This document is intentionally **deep, technical, and structured**. It is designed to be:

* Handed to an engineer without additional context
* Published directly as a GitHub research artifact
* Used as a reference for low‑resource LLM deployment and reasoning control

Nothing here is hand‑wavy. We describe:

* What actually happens inside an LLM at inference time
* What *can* and *cannot* be tuned on ~1 GB–class systems
* How to suppress wasted memory and reasoning overhead
* How to reason about model choice scientifically

---

# PART I — HOW TO CUSTOMIZE & FINE‑TUNE REASONING (WITHOUT GPU)

## 1. What “Reasoning” Actually Is (Internals)

An LLM does **not** reason symbolically.
What we call *reasoning* is:

> **Token‑by‑token probability shaping over an internal latent space**

### Inference Pipeline

1. Input tokens → embedding vectors
2. Embeddings pass through transformer layers
3. Each layer refines attention + hidden state
4. Final logits → probability distribution over next token
5. Sampling strategy selects the next token

### Implications

“Better reasoning” means:

* Reducing entropy
* Reducing irrelevant attention
* Biasing token paths toward structured output

You **cannot** change transformer weights on low RAM systems.
You **can** change how the model explores its probability space.

That is what you control.

---

## 2. Inference‑Time Controls (What You *Can* Tune)

### 2.1 Temperature — Entropy Control

```text
temperature → controls randomness
```

**Mathematical form**:

```
softmax(logits / temperature)
```

Effects:

* ↓ temperature → sharper probability peaks
* ↑ temperature → flatter distribution

**Recommended values (CPU‑only reasoning)**:

| Task                 | Temperature |
| -------------------- | ----------- |
| Logic / explanations | 0.1–0.2     |
| Code                 | 0.05–0.15   |
| Exploration          | 0.3–0.5     |

High temperature increases branching, CPU cost, and logical instability.

---

### 2.2 Top‑p — Nucleus Sampling

```text
top_p → cumulative probability cutoff
```

Mechanism:

* Sort tokens by probability
* Keep smallest set whose cumulative probability ≥ top_p
* Sample only from that set

**Recommended**:

```text
top_p = 0.8–0.9
```

Benefits:

* Eliminates low‑probability nonsense tokens
* Reduces hallucinations
* Reduces token search space (CPU win)

---

### 2.3 Repeat Penalty — Loop Suppression

```text
repeat_penalty = 1.1–1.25
```

Internals:

* Penalizes logits of previously emitted tokens
* Prevents degeneracy loops

Failure modes:

* Too high → broken grammar
* Too low → infinite repetition

---

### 2.4 Context Length — The Memory Dominator

```text
num_ctx = context window (tokens)
```

Approximate memory scaling:

```
O(num_ctx × layers × hidden_dim)
```

On ~1–2 GB systems:

* **1024 tokens = hard upper bound**
* 512 tokens = optimal

> Smaller context often yields *better reasoning per token*.

---

## 3. System Prompt Engineering (Highest ROI Lever)

### Why System Prompts Matter More Than Data

System prompts:

* Are injected **before user input**
* Bias *every* transformer layer
* Cost **zero additional RAM**

**Good example**:

```text
SYSTEM:
You reason step‑by‑step internally.
Output concise, verifiable explanations.
Avoid speculation.
Prefer formal logic and concrete examples.
```

**Bad example**:

```text
Be very smart and detailed.
```

Why:

* Vague prompts create diffuse attention
* Specific constraints collapse the probability space

---

## 4. “Adding Data” Without Training

You **cannot fine‑tune weights** on 1–4 GB RAM systems.

Instead, you use **retrieval‑augmented prompting (RAP)**.

### 4.1 Static Knowledge Injection

```text
SYSTEM:
Reference:
- CPU cache hierarchy: L1, L2, L3
- Memory latency dominates reasoning speed
- Deterministic decoding preferred

Now explain…
```

Effects:

* Overrides pretraining bias
* Costs tokens, not RAM

---

### 4.2 Domain‑Specific Prompt Templates

Create role‑specific profiles:

* `qwen‑explain`
* `qwen‑logic`
* `qwen‑codeonly`

Each profile enforces:

* Fixed reasoning constraints
* No verbosity drift

This is **behavioral fine‑tuning**, not weight tuning.

---

# PART II — MEMORY, CACHE, AND PRODUCTION MAINTENANCE

## 5. How Ollama Uses Memory

### Memory Components

1. Model weights (static)
2. KV cache (grows with context)
3. Temporary buffers (attention, matmul)

The dominant cost is the **KV cache**.

---

## 6. KV Cache — What It Is & How to Control It

### What Is KV Cache?

For each token, the model stores:

* Key vectors
* Value vectors

Used for attention reuse.

Memory usage:

```
KV ≈ num_layers × hidden_dim × num_ctx × 2
```

### Control Strategies

* Reduce `num_ctx`
* Avoid multi‑turn chats
* Restart Ollama periodically

```bash
ollama stop qwen‑optimal
ollama run qwen‑optimal
```

This flushes the KV cache.

---

## 7. Suppressing Excess Reasoning Memory

### 7.1 Avoid Chain‑of‑Thought Leakage

Do **not** request:

```text
Explain your reasoning in detail
```

Instead:

```text
Give the conclusion and a short justification
```

Why:

* Internal reasoning ≠ output tokens
* Verbose output increases KV cache

---

### 7.2 Hard Output Constraints

```text
Respond in ≤150 tokens.
No markdown.
No repetition.
```

This caps memory growth deterministically.

---

## 8. Code & Cache Hygiene

### Disk Cache Location

```bash
~/.ollama/models
```

Check usage:

```bash
du -sh ~/.ollama/models/*
```

Remove unused models:

```bash
ollama rm modelname
```

Models are memory‑mapped, but disk pressure still matters.

---

# PART III — HOW LLMs WORK (PRODUCTION VIEW)

## 9. Execution Pipeline

```
Tokens → Embeddings
      → Transformer Layers (N)
      → Attention + FFN
      → Logits
      → Sampling
      → Output
```

> Reasoning quality = signal‑to‑noise ratio in attention layers

Inference tuning improves **signal**, not intelligence.

---

## 10. Choosing an LLM for ~1 GB RAM

### Hard Constraints

| Constraint   | Requirement |
| ------------ | ----------- |
| AVX          | None        |
| Parameters   | ≤1.5B       |
| Quantization | Q4 or Q5    |
| Context      | ≤1024       |

### Known Good Models

| Model     | Notes                        |
| --------- | ---------------------------- |
| Qwen 1.5B | Best reasoning per parameter |
| TinyLlama | Faster, weaker logic         |
| Phi‑2     | Strong logic, fragile        |

Avoid:

* Mixture‑of‑experts models
* Long‑context architectures
* Chat‑tuned large models

---

## 11. Long‑Term Memory Suppression Strategy

1. Short sessions
2. Stateless prompts
3. Restart daemon
4. Separate profiles per task
5. Never multitask models

---

# PART IV — RESEARCH PAPER

# Quantifying Preference Structures for Low‑Resource Language Model Deployment

## Abstract

This paper introduces a framework for quantifying user and system preferences when deploying large language models under severe resource constraints. We demonstrate that reasoning quality is not solely dependent on model size but on the alignment between preference weighting, inference controls, and architectural limitations. We propose a preference vector formalism and validate it on CPU‑only systems with <2 GB RAM.

---

## 1. Introduction

Most LLM evaluations optimize for:

* Accuracy
* Fluency
* Benchmark scores

Low‑resource environments demand different priorities:

* Determinism
* Memory efficiency
* Latency stability
* Predictable degradation

Preference misalignment is the dominant failure mode on constrained systems.

---

## 2. Preference Vector Definition

```
P = {R, D, M, L, V}
```

Where:

* R = Reasoning depth
* D = Determinism
* M = Memory footprint
* L = Latency
* V = Verbosity

Each ∈ [0,1]

---

## 3. Model Compatibility Function

```
C(model, system) = Σ wi · Pi
```

For a ~1 GB system:

| Preference  | Weight |
| ----------- | ------ |
| Memory      | 0.35   |
| Determinism | 0.25   |
| Latency     | 0.20   |
| Reasoning   | 0.15   |
| Verbosity   | 0.05   |

---

## 4. Empirical Observations

* Smaller aligned models outperform larger misaligned ones
* Lower entropy decoding improves reasoning stability
* Reduced context improves explanation coherence

---

## 5. Discussion

Reasoning is **preference‑constrained sampling**, not emergent intelligence.

Systems exposing preference controls outperform systems relying on scale alone.

---

## 6. Conclusion

Low‑resource LLM deployment is a preference optimization problem, not a hardware limitation problem. Proper tuning yields stable, interpretable reasoning under extreme constraints.

---
