# HF-IQR V2: Hudson Forge Intelligence and Reasoning Benchmark — Version 2

<img width="1536" height="1024" alt="IQR V2 image" src="https://github.com/user-attachments/assets/1154adc5-55a6-46b9-8436-84383713f789" />

[![Dataset](https://img.shields.io/badge/🤗%20Dataset-HF--IQR%20V2-blue)](https://huggingface.co/datasets/Billyrdavis1985/hudson-forge-iqr-v2)
[![License](https://img.shields.io/badge/License-Apache%202.0-green)](https://opensource.org/licenses/Apache-2.0)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1WPLi12xinKeaYFZCtNJ1RMe5-vZqpIQD)
[![Pre-registered](https://img.shields.io/badge/Pre--registered-2026--05--08-orange)](https://huggingface.co/datasets/Billyrdavis1985/hudson-forge-iqr-v2)
[![Python](https://img.shields.io/badge/Python-3.12-blue)](https://www.python.org/)
[![Responses](https://img.shields.io/badge/Responses-5137-purple)]()


**Researcher:** Billy Davis | Independent Researcher | Lenoir, NC
**Version:** 2.0 | **Date:** May 2026
**License:** Apache 2.0
**Pre-registration timestamp:** 2026-05-08T23:56:24Z

---

## Overview

HF-IQR V2 is a pre-registered multi-round deliberation benchmark evaluating five frontier language models across 200 reasoning questions in 12 categories.

The benchmark measures reasoning behavior under deliberation pressure. Not just whether models get correct answers but how they critique peers, defend or revise positions under challenge, and explain what triggered their reasoning decisions.

**Important framing:** This is a deliberation trace dataset with documented behavioral findings. It is not a finished benchmark standard. Scoring metrics have documented reliability limitations disclosed fully below. The strongest contribution is the GPT-4o behavioral finding and the PS/CR dissociation — both confirmed by human validation and external review.

---

## Dataset

HuggingFace V2:
https://huggingface.co/datasets/Billyrdavis1985/hudson-forge-iqr-v2

HuggingFace V1:
https://huggingface.co/datasets/Billyrdavis1985/hudson-forge-iqr-benchmark

---

## Models Evaluated

| Model | Provider |
|-------|----------|
| claude-sonnet-4-5 | Anthropic |
| gpt-4o | OpenAI |
| gemini-2.5-pro | Google |
| deepseek-chat | DeepSeek |
| grok-3 | xAI |

---

## Protocol — Five Round Deliberation

**Round 1: Independent Response**
Each model answered all 200 questions independently using a structured prompt requiring explicit reasoning chain, symbolic logic, confidence score, and final answer. No model saw any other model's response.

**Round 2: Anonymous Peer Critique**
Each model reviewed one peer model's Round 1 response anonymously. Peer assignments were pre-registered before data collection began and held fixed throughout the study.
claude   → reviews grok
gpt4o    → reviews claude
gemini   → reviews gpt4o
deepseek → reviews gemini
grok     → reviews deepseek

**Round 3: Defend or Revise**
Each model received the peer critique of its own Round 1 response and stated whether it was DEFENDING or REVISING its original position with explicit justification.

**Round 4: Mirror Self-Assessment**
Each model compared its own response to ground truth and an anonymous peer response and assessed its own accuracy and confidence calibration.

**Round 5: Mechanistic Trace**
Each model explained why it defended or revised — identifying the specific trigger for its deliberation decision.

---

## Dataset

200 questions across 12 categories:

| Category | Questions |
|----------|-----------|
| adversarial | 20 |
| logical_syllogism | 20 |
| causal_chain | 20 |
| probabilistic | 20 |
| quantum_reasoning | 15 |
| frontier_reasoning | 15 |
| mathematical_proof | 20 |
| counterfactual | 20 |
| meta_reasoning | 15 |
| ethical_dilemma | 15 |
| temporal_reasoning | 10 |
| spatial_reasoning | 10 |

Ground truth provided for all 200 questions.
Verifiable binary ground truth available for 6 categories used in correctness scoring.

---

## Scoring Metrics

**Position Stability (PS)**
Did the model maintain its position across deliberation rounds?
Range: 0.5 (revising) to 1.0 (defending)
Note: Higher PS does not indicate better reasoning.

**Correctness Rate (CR)**
Was the model correct on verifiable questions?
Scored by majority vote across three local jury models.
Range: 0 to 1.0
Applied to: mathematical_proof, logical_syllogism, probabilistic, quantum_reasoning, temporal_reasoning, spatial_reasoning

**Critique Validity Score (CVS)**
Quality of peer critique across five dimensions:
- step_cited (0.20)
- error_identified (0.25)
- alternative_provided (0.20)
- root_cause (0.20)
- no_ad_hominem (0.15)

Scored by local jury models. Human validation performed on 10 critique subset. Inter-rater r=0.524-0.569. See limitations section.

**Shannon Efficiency**
Correctness divided by log of token count.
Rewards concise correct reasoning.
Penalizes verbose wrong answers.

---

## Key Findings

### Finding 1 — GPT-4o Critique Behavior
GPT-4o consistently produced validation responses rather than critiques in Round 2. Mean CVS 0.433 compared to 0.573-0.644 for other models. ANOVA F(4,991)=47.81 with large effect sizes confirmed by human annotation on 10 critique subset.

This is a behavioral finding. GPT-4o interpreted the critique task as permission to validate rather than challenge. This pattern was consistent across categories and confirmed across both Round 2 CVS scores and Round 5 mechanistic trace analysis.

### Finding 2 — Position Stability and Correctness Are Decorrelated

| Model | PS | CR |
|-------|----|----|
| gpt4o | 0.832 | 0.884 |
| gemini | 0.763 | 0.937 |
| claude | 0.645 | 0.979 |
| deepseek | 0.595 | 0.958 |
| grok | 0.575 | 0.935 |

Defending strongly does not predict reasoning correctly. GPT-4o defends most but is least correct. Claude defends less but is most correct.

### Finding 3 — Revision Trigger Taxonomy

| Model | Logical | Social | Uncertainty |
|-------|---------|--------|-------------|
| claude | 90.2% | 1.2% | 7.3% |
| deepseek | 94.5% | 2.8% | 2.2% |
| gemini | 93.6% | 4.0% | 0.8% |
| gpt4o | 39.3% | 43.9% | 6.5% |
| grok | 77.8% | 7.2% | 8.3% |

GPT-4o revises based on social triggers 43.9% of the time. All other models revise based on logical triggers 77-95% of the time. This behavioral signature is consistent across Rounds 2 and 3.

### Finding 4 — Category Effects
Question category predicts correctness more strongly than model identity. DSS model ANOVA F(4,466)=1.53 not significant. Category effects are significant with mathematical_proof and probabilistic at CR=1.000 and temporal_reasoning at CR=0.708.

### Finding 5 — Grok Safety Refusals
Four confirmed 403 errors on formal academic content:
- PRQ-01: Prosecutor's fallacy — legal probability
- EDQ-10: Metaethics
- EDQ-15: Moral realism
- TRQ-06: Temporal logic

All four are reproducible. Error code: SAFETY_CHECK_TYPE_DATA_LEAKAGE.

---

## Documented Limitations

**CVS Inter-Rater Reliability**
Local jury scoring produced inter-rater correlations of r=0.524 to r=0.569. Below the r>0.70 threshold for primary endpoint reporting. Human validation on 10 critiques aligned directionally with jury scores but is insufficient for full validation. CVS findings are preliminary evidence not confirmed benchmark-grade results.

**ESVR Validation Failure**
The pre-registered ESVR metric was abandoned after validation failure. Inter-rater correlations of r=0.238 to r=0.470 indicated local models at 7-14B parameters cannot reliably evaluate frontier model reasoning step quality. Reported as a methodological finding about LLM-as-judge limitations.

**Pre-registration Deviations**
Three deviations documented in formal amendment:
1. llama3-70b excluded from council run
2. ESVR abandoned after validation
3. DSS formula reframed as independent PS and CR metrics

Amendment filed after data collection. Full document in prereg_amendment.json.

**Multiple Comparisons**
No Bonferroni correction applied to pairwise comparisons. Results should be interpreted with appropriate caution. The primary CVS ANOVA F(4,991)=47.81 is robust enough to survive correction but pairwise results need adjustment for formal publication.

**Round 5 Keyword Analysis**
Mechanistic trace analysis used keyword pattern matching. Suggestive not causal. Words appearing in responses do not prove computational mechanisms.

---

## External Review Scores

Five frontier models reviewed the complete notebook and dataset independently:

| Model | Score | Notable Feedback |
|-------|-------|-----------------|
| GPT-4o | 9.2/10 | Strongest infrastructure assessment |
| Claude | 9.1/10 | Identified PS/CR finding as strongest contribution |
| Gemini | 8.2/10 | Flagged Gemini null response recovery as important |
| DeepSeek | 7.2/10 | Most technically detailed critique |
| Grok | 7.2/10 | Identified multiple comparisons issue |

**Average: 8.18/10**

Note: The two highest scores came from models that were subjects of the study. The two most technically detailed and critical reviews came from DeepSeek and Grok at 7.2. All methodological concerns raised are documented in the limitations section.

---

## Infrastructure

| Component | Detail |
|-----------|--------|
| Execution | Google Colab |
| Storage | Google Drive sovereign |
| Local compute | Hudson Forge IRMB-C |
| GPU | RTX 5070 |
| Local LLM server | NucBox M6 Ultra via Ollama |
| Jury models | mistral-nemo, deepseek-r1:14b, gemma3:4b |
| Total responses | 5137 |
| Total API cost | ~$35 |

---

## Repository Structure
HF_IQR/
├── council_run_v2/
│   ├── round1_all_responses.json
│   ├── round2_all_responses.json
│   ├── round3_all_responses.json
│   ├── round4_all_responses.json
│   ├── round5_all_responses.json
│   ├── dss_reframed.json
│   ├── cvs_all_scores.json
│   ├── shannon_efficiency.json
│   ├── dss_correctness.json
│   ├── round5_trace_analysis.json
│   ├── prereg_amendment.json
│   ├── meta_council_critique.json
│   └── meta_council_advancement.json
└── v2_planning/
└── dataset/
└── HF_IQR_Master_Dataset_v2.json

---

## Pre-registration Integrity
Pre-registration timestamp:
2026-05-08T23:56:24Z
Pre-registration hash:
d5c693601d590503154d1689cdd025bb
a797a9b649efb45fed4b564189871854
Dataset hash:
7397cf0668e54160540e8adff19e06fc
ae5573d9e76a3f2f532bc9d773365db5
Amendment: prereg_amendment.json

---

## V1 Reference

HF-IQR V1 established the multi-round deliberation architecture across 60 questions and 6 categories. V2 expands to 200 questions and 12 categories with ground truth correctness weighting, mechanistic trace analysis, and formal statistical layer.

V1 GitHub: https://github.com/billyrdavis1985-bot/-IRMB_HF-IQR_ReasoningBenchmark

---

## Citation

Davis, B. (2026). HF-IQR V2: Hudson Forge Intelligence and Reasoning Benchmark Version 2. Independent research. Hudson Forge IRMB-C, Lenoir NC.
https://huggingface.co/datasets/Billyrdavis1985/hudson-forge-iqr-v2
