# -Local-Uncensored-AI-Setup-
# Ollama + LLaMA 2 — Local Setup (GitHub-ready)

A step-by-step guide to install **Ollama**, pull **LLaMA 2** models, run them locally, and upload everything to your GitHub repository. Includes example scripts you can copy directly into a repo.

---

## ⚙️ Overview

This repository shows how to run LLaMA 2 locally using Ollama. It covers installation (macOS / Linux / Windows), pulling models, running them via the CLI or HTTP API, basic troubleshooting, legal notes, and how to push the project to GitHub.

---

## ✅ Requirements

* **Operating System:** macOS, Linux, or Windows (WSL recommended for Windows power users).
* **Hardware (recommended):**

  * 7B models: 8–16 GB RAM and ~10–20 GB disk.
  * 13B models: 16+ GB RAM.
  * 70B models: 64+ GB RAM and large disk space.
* **Network:** reliable internet for initial model download.
* **Optional (GPU):** Nvidia GPU with CUDA/cuDNN — greatly improves performance for larger models.

---

## 1) Install Ollama

### macOS / Linux (quick):

```bash
# Run the official install script (Linux / WSL / macOS)
curl -fsSL https://ollama.com/install.sh | sh
```

After installation, verify with:

```bash
ollama version
```

### Windows:

* Download the Ollama installer from the Ollama website and run it, or use WSL and run the curl command above.

---

## 2) Pull / Download LLaMA 2

Pick the size you need (7B, 13B, ...). Example (7B chat variant):

```bash
# Download 7B chat variant
ollama pull llama2:7b-chat

# Or download 13B chat variant
ollama pull llama2:13b-chat
```

> If you run `ollama run llama2` and the model isn't present, Ollama will pull it automatically.

---

## 3) Run the model locally

### CLI (interactive):

```bash
# Run the 7B chat model interactively
ollama run llama2:7b-chat

# Or run the default llama2 model
ollama run llama2
```

This starts a local API server (default `http://localhost:11434`) and opens an interactive prompt.

### Serve in background (example):

```bash
ollama serve &
# then use the API (see next section)
```

---

## 4) Query the model via HTTP API

Example using `curl` (JSON body):

```bash
curl -X POST http://localhost:11434/api/generate \
  -H "Content-Type: application/json" \
  -d '{"model":"llama2","prompt":"Write a short introduction to Istanbul.","max_tokens":120}'
```

Python example (`run_demo.py`):

```python
# run_demo.py
import requests
import json

resp = requests.post(
    "http://localhost:11434/api/generate",
    json={
        "model": "llama2",
        "prompt": "Write a short introduction to Istanbul.",
        "max_tokens": 120
    }
)
print(json.dumps(resp.json(), indent=2))
```

---

## 5) Example helper scripts

### setup.sh (Linux/macOS)

```bash
#!/usr/bin/env bash
set -e

# Install ollama (if not installed)
if ! command -v ollama >/dev/null 2>&1; then
  echo "Installing Ollama..."
  curl -fsSL https://ollama.com/install.sh | sh
else
  echo "Ollama already installed"
fi

# Pull LLaMA 2 (7B chat) — change to 13b-chat if you want
ollama pull llama2:7b-chat

echo "Done. Use 'ollama run llama2:7b-chat' to start an interactive session."
```

### run_demo.py (same as API example above)

---

## 6) Troubleshooting & tips

* **Not enough RAM / disk:** use smaller model variants/quantized variants (if available).
* **Download interruptions / progress revert:** network or registry hiccups have been reported — retry `ollama pull` or try a different network.
* **If you already have GGUF/.pth files:** you can import them into Ollama (community guides exist) or run with a local Modelfile — see Ollama docs.
* **GUI:** Ollama has a desktop app on Windows/macOS for simpler interactions; CLI is recommended for development and scripting.

---


* **Do not expose** your Ollama server (`localhost:11434`) to the public internet without authentication and hardening. There have been reports of misconfigured/ exposed LLM servers in the wild — secure your machine and firewall.




---

*Happy local modeling — tell me which OS you want full scripts for (Windows PowerShell, macOS, or Linux) and I’ll add them to the repo.*
