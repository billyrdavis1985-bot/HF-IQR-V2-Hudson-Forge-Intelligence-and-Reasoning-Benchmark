# Reproducibility Guide

## Overview

This document provides complete instructions
for reproducing the HF-IQR V2 council run
and scoring pipeline from scratch.

---

## Requirements

### API Keys Required

| Service | Models | Estimated Cost |
|---------|--------|----------------|
| Anthropic | claude-sonnet-4-5 | ~$8 |
| OpenAI | gpt-4o | ~$12 |
| Google | gemini-2.5-pro | ~$6 |
| DeepSeek | deepseek-chat | ~$3 |
| xAI | grok-3 | ~$6 |

**Total estimated cost: ~$35**

### Local Infrastructure Required

| Component | Minimum Spec | Used in Study |
|-----------|-------------|---------------|
| GPU machine | 8GB VRAM | RTX 5070 |
| Local LLM server | 16GB RAM | NucBox M6 Ultra 32GB |
| Ollama | Latest version | v0.3+ |

### Jury Models Required

Install via Ollama:
```bash
ollama pull mistral-nemo
ollama pull deepseek-r1:14b
ollama pull gemma3:4b
```

### Python Dependencies

```bash
pip install anthropic
pip install openai
pip install google-generativeai
pip install requests
pip install numpy
pip install json
pip install hashlib
```

---

## Reproduction Steps

### Step 1 — Environment Setup
Open HF_IQR_Phase0_V2_Prep.ipynb in
Google Colab. Run all setup cells.
Mount Google Drive. Set API keys.

### Step 2 — Dataset Verification
Load HF_IQR_Master_Dataset_v2.json
from HuggingFace. Verify dataset hash:
