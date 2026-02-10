# Ollama Low-RAM Coding Assistant (Fully Local, Fully Verified)

This repository documents a **fully local AI coding environment** designed to run on **extremely low-resource hardware**.

This is **not theoretical**.  
Every step was tested and verified on:

- CPU: Intel Celeron N3450 (4 threads, 1.1–2.2 GHz)
- Architecture: x86_64 (NO AVX / AVX2)
- RAM: 4 GB installed (~1.8 GB usable)
- Storage: 256 GB NVMe SSD
- OS: Arch Linux
- Swap: ZRAM
- Editor: Neovim
- AI Runtime: Ollama
- Model: Qwen 2.5 Coder 1.5B

---

## Why This Project Exists

Most “local AI coding” guides assume:
- 8–32 GB RAM
- AVX2/AVX512
- GPU acceleration

This project proves you **do not need that**.

Goals:
- Fully offline
- CPU-only
- Stable on ~1.8 GB RAM
- Deterministic, reliable outputs
- Integrated directly into Neovim
- No Electron, no browser IDEs

---

## Repository Layout

ollama-lowram-coding/
├── README.md
├── CHEATSHEET.md
├── setup/
│ ├── install_ollama.sh
│ ├── zram_setup.sh
│ └── model_profiles/
│ ├── qwen-n3450/Modelfile
│ ├── qwen-codeonly/Modelfile
│ ├── qwen-explain/Modelfile
│ └── qwen-optimal/Modelfile
└── neovim_integration/
└── init.lua
