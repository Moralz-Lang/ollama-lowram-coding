# Ollama Low-RAM Coding Assistant

[![Repo Size](https://img.shields.io/github/repo-size/Moralz-Lang/ollama-lowram-coding)](https://github.com/Moralz-Lang/ollama-lowram-coding)
[![Issues](https://img.shields.io/github/issues/Moralz-Lang/ollama-lowram-coding)](https://github.com/Moralz-Lang/ollama-lowram-coding/issues)
[![Stars](https://img.shields.io/github/stars/Moralz-Lang/ollama-lowram-coding)](https://github.com/Moralz-Lang/ollama-lowram-coding/stargazers)

A **fully local, low-RAM AI coding assistant setup** using Ollama + Qwen 2.5 Coder 1.5B — optimized for CPU-only systems, Arch Linux, and Neovim. :contentReference[oaicite:1]{index=1}

---

## 📌 Table of Contents

1. [About This Project](#about-this-project)  
2. [Hardware & System Requirements](#hardware-system-requirements)  
3. [Setup & Installation](#setup-installation)  
   * [ZRAM Swap (Critical)](#zram-swap-critical)  
   * [Install Ollama](#install-ollama)  
   * [Pull Base Model](#pull-base-model)  
4. [Model Profiles](#model-profiles)  
   * [qwen-n3450](#qwen-n3450)  
   * [qwen-codeonly](#qwen-codeonly)  
   * [qwen-explain](#qwen-explain)  
   * [qwen-optimal](#qwen-optimal)  
5. [Neovim Integration](#neovim-integration)  
6. [Parameter Tuning Guide](#parameter-tuning-guide)  
7. [Monitoring & Hygiene](#monitoring-hygiene)  
8. [Further Reading & Research](#further-reading-research)  
9. [Pros & Cons](#pros-cons)  
10. [Better Hardware Recommendations](#better-hardware-recommendations)  
11. [Contributing](#contributing)

---

## 📌 About This Project

This repository shows how to configure and deploy an AI coding assistant **fully offline** on machines with:

- CPU-only (no GPU required)  
- ~1.8 GB usable RAM  
- No AVX/AVX2  
- SSD/NVMe  
- Arch Linux + Neovim

The full explanation — from hardware choices to inference parameter tuning — is documented in [/Low-Resource LLM Reasoning & Preference Optimization.md](./Low-Resource%20LLM%20Reasoning%20%26%20Preference%20Optimization.md). :contentReference[oaicite:2]{index=2}

---

## 📌 Hardware & System Requirements

Key constraints tested on:

- **CPU:** Intel Celeron N3450 (no AVX)  
- **RAM:** ~1.8 GB usable  
- **Storage:** NVMe SSD  
- **OS:** Arch Linux  
- **Editor:** Neovim  
- **AI runtime:** Ollama  
- **Model:** Qwen 2.5 Coder 1.5B

Full details: [HardwareVerification.md](./HardwareVerification.md) :contentReference[oaicite:3]{index=3}

---

## 📌 Setup & Installation

### ZRAM Swap (Critical)

ZRAM prevents out-of-memory crashes.

> See: [3. ZRAM Swap(Critcal).md](./3.%20ZRAM%20Swap%28Critcal%29.md) :contentReference[oaicite:4]{index=4}

---

### Install Ollama

```bash
bash setup/install_ollama.sh
