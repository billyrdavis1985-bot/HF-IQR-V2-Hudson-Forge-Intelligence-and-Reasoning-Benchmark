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
7397cf0668e54160540e8adff19e06fc
ae5573d9e76a3f2f532bc9d773365db5

### Step 3 — Council Run
Open HF_IQR_V2_Council_Run.ipynb.
Run cells in order:
1. Setup and imports
2. API client initialization
3. Checkpoint manager
4. Round 1 — Independent response
5. Round 2 — Peer critique
6. Round 3 — Defend or revise
7. Round 4 — Self assessment
8. Round 5 — Mechanistic trace

Each round saves checkpoints to Drive.
If interrupted resume from checkpoint.

### Step 4 — Scoring

**DSS Scoring:**
Run DSS correctness cells.
Requires local Ollama jury models running.
Set NGROK_URL to your tunnel address.

**CVS Scoring:**
Run CVS validation sample first (40 questions).
Verify inter-rater correlations before full run.
Run full CVS by category using guard cells.

**Shannon Efficiency:**
Run after DSS correctness is complete.
No additional API calls required.

### Step 5 — Analysis
Run ANOVA cells.
Run bootstrap CI cells.
Run Round 5 trace analysis cells.

### Step 6 — Verification
Compare your results to published dataset
on HuggingFace. Key numbers to verify:

| Metric | Expected Value |
|--------|----------------|
| CVS ANOVA F-stat | 47.81 |
| GPT-4o mean CVS | 0.433 |
| DeepSeek mean CVS | 0.644 |
| Claude CR | 0.979 |
| GPT-4o PS | 0.832 |

---

## Known Issues and Workarounds

**Gemini null responses**
Gemini must use native google-generativeai
client not OpenAI compatible wrapper.
148 null responses occurred in original
run due to wrong client. Fixed in notebook.

**Grok safety refusals**
Four questions produce 403 errors with Grok:
PRQ-01, EDQ-10, EDQ-15, TRQ-06.
These are expected and documented.
Do not retry — log as safety refusal.

**Session state loss in Colab**
Rebuild sample_ids at the top of every
CVS scoring cell using the guard cell pattern.
Failure to do this causes wrong questions
to be included in scoring runs.

**NGROK tunnel timeout**
Local jury scoring requires active NGROK tunnel.
If tunnel times out restart and update NGROK_URL
before resuming scoring.

---

## Pre-registration Verification

To verify the pre-registration timestamp:

```python
import hashlib
import json

with open('HF_IQR_V2_Preregistration.json',
          'r') as f:
    content = f.read()

hash_value = hashlib.sha256(
    content.encode()).hexdigest()

expected = (
    "d5c693601d590503154d1689cdd025bb"
    "a797a9b649efb45fed4b564189871854"
)

print(f"Hash matches: {hash_value == expected}")
```

---

## Differences From Published Run

If your reproduction produces different numbers
the most likely causes are:

Model version differences
API models update without notice
claude-sonnet-4-5 may behave differently
on a later date
Temperature not fixed at 0
Responses are non-deterministic
Expect variation in individual responses
Aggregate statistics should be similar
Jury model version differences
Ollama model updates may affect scoring
Use exact model versions listed above


---

## Contact

Billy Davis | Independent Researcher
Hudson Forge IRMB-C | Lenoir NC
LinkedIn: Billy Davis

GitHub:
https://github.com/billyrdavis1985-bot/
HF-IQR-V2-Hudson-Forge-Intelligence-and-Reasoning-Benchmark

HuggingFace:
https://huggingface.co/datasets/
Billyrdavis1985/hudson-forge-iqr-v2

