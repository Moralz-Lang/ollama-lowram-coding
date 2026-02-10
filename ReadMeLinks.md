# Ollama Low‑RAM Coding Assistant

[![Repo Size](https://img.shields.io/github/repo-size/Moralz-Lang/ollama-lowram-coding)](https://github.com/Moralz-Lang/ollama-lowram-coding)
[![Issues](https://img.shields.io/github/issues/Moralz-Lang/ollama-lowram-coding)](https://github.com/Moralz-Lang/ollama-lowram-coding/issues)
[![Stars](https://img.shields.io/github/stars/Moralz-Lang/ollama-lowram-coding)](https://github.com/Moralz-Lang/ollama-lowram-coding/stargazers)

A **fully local, low‑RAM AI coding assistant setup** using Ollama + Qwen 2.5‑Coder 1.5B — designed to run on CPU‑only systems with limited memory.

---

## 📌 Table of Contents

1. [About This Project](#about-this-project)  
2. [Hardware & System Requirements](#hardware--system-requirements)  
3. [Setup & Installation](#setup--installation)  
   * [ZRAM Swap (Critical)](#zram-swap-critical)  
   * [Install Ollama](#install-ollama)  
   * [Model Selection Logic](#model-selection-logic)  
4. [Model Profiles](#model-profiles)  
5. [Neovim Integration](#neovim-integration)  
6. [Parameter Tuning Guide](#parameter-tuning-guide)  
7. [Monitoring & Hygiene](#monitoring--hygiene)  
8. [Further Reading & Research](#further-reading--research)  
9. [Pros & Cons](#pros--cons)  
10. [Better Hardware Recommendations](#better-hardware-recommendations)  
11. [Final Verification](#final-verification)  
12. [Contributing](#contributing)

---

## 📌 About This Project

This repository shows how to build and run a fully local coding assistant using Ollama on machines with:

- CPU‑only (no GPU)
- Limited RAM (~1.8 GB usable)
- No AVX support
- Arch Linux + Neovim

The full technical explanation is available in:

➡️ **[Low‑Resource LLM Reasoning & Preference Optimization.md](./Low‑Resource%20LLM%20Reasoning%20%26%20Preference%20Optimization.md)**

---

## 📌 Hardware & System Requirements

Validated configuration:

- **CPU:** Intel Celeron N3450 (no AVX / AVX2)  
- **RAM:** ~1.8 GB usable  
- **Storage:** SSD / NVMe  
- **OS:** Arch Linux  
- **Editor:** Neovim  
- **AI Runtime:** Ollama  
- **Model:** Qwen 2.5‑Coder 1.5B

➡️ Detailed analysis in: **[HardwareVerification.md](./HardwareVerification.md)**

---

## 📌 Setup & Installation

### 🧠 ZRAM Swap (Critical)

Low‑RAM systems require ZRAM to prevent crashes:

➡️ **[3. ZRAM Swap(Critcal).md](./3.%20ZRAM%20Swap%28Critcal%29.md)**

---

### 🛠 Install Ollama

Step‑by‑step install of the Ollama runtime:

➡️ **[4. Installing Ollama.md](./4.%20Installing%20Ollama.md)**

Official docs: https://github.com/ollama/ollama :contentReference[oaicite:1]{index=1}

---

### 📊 Model Selection Logic

Why Qwen 2.5‑Coder was chosen and evaluation of alternatives:

➡️ **[5. Model Selection Logic.md](./5.%20Model%20Selection%20Logic.md)**

---

## 📌 Model Profiles

Profiles define different behaviors without duplicating weights:

➡️ **[6. Model Profiles (Behavioral Control).md](./6.%20Model%20Profiles%20%28Behavioral%20Control%29.md)**

Profiles included:
- qwen‑n3450
- qwen‑codeonly
- qwen‑explain
- qwen‑optimal

---

## 📌 Neovim Integration

How to connect Ollama to Neovim workflows:

➡️ **[7. Neovim Integration.md](./7.%20Neovim%20Integration.md)**

---

## 📌 Parameter Tuning Guide

Optimizing temperature, top_p, repeat_penalty, and context length:

➡️ **[8. Parameter Tuning Guide.md](./8.%20Parameter%20Tuning%20Guide.md)**

---

## 📌 Monitoring & Hygiene

Commands for monitoring memory, disk, and cleanup:

➡️ **[9. Monitoring Commands](./9.%20Monitoring%20Commands)**

---

## 📌 Further Reading & Research

In‑depth research and preference optimization:

➡️ **[Low‑Resource LLM Reasoning & Preference Optimization.md](./Low‑Resource%20LLM%20Reasoning%20%26%20Preference%20Optimization.md)**

---

## 📌 Pros & Cons

Honest trade‑offs of this approach:

➡️ **[10. Pros & Cons.md](./10.%20Pros%20%26%20Cons.md)**

---

## 📌 Better Hardware Recommendations

How the project can scale with more RAM, CPU, or GPU:

➡️ **[11. Better Hardware Recommendations.md](./11.%20Better%20Hardware%20Recommendations.md)**

---

## 📌 Final Verification

Summary of verification steps and outcomes:

➡️ **[12. Final Verification.md](./12.%20Final%20Verification.md)**

---

## 📌 Contributing

Contributions are welcome!  
Open an issue or submit a pull request.

