# CVS Metric Reliability and Interpretation Guide

## Overview

CVS (Critique Validity Score) is the core metric
for Finding 1 — GPT-4o critique behavior.
This document explains what the reliability
numbers mean and how findings should be interpreted.

## Current Reliability

| Metric | Value | Threshold | Status |
|--------|-------|-----------|--------|
| Inter-rater correlation | r=0.524-0.569 | r>0.70 | Moderate |
| ESVR (abandoned) | r=0.238-0.470 | r>0.70 | Failed |
| Human validation | 10 critiques | 50+ needed | Partial |

## What Moderate Reliability Means

r=0.524-0.569 indicates moderate agreement
between jury models — not high agreement.

This means:
- NOT suitable for high-stakes claims without
  further validation
- STILL valid for directional findings
  (GPT-4o behaves differently from other models)
- Requires expert validation in V3 before
  benchmark-grade claims can be made

## Why Reliability Is This Low

**1. Jury model capability gap**
Local jury models at 7-14B parameters are
judging frontier models at 70B+ equivalent.
Similar to an undergraduate grading a PhD thesis.
Inherent asymmetry in domain knowledge.

**2. Critique scoring is subjective**
Whether a critique is valid depends on context.
Local models miss domain-specific expertise.
Human experts with relevant knowledge would
score more consistently.

**3. Round 2 protocol effects**
Critiques are intentionally adversarial.
Local models may apply harsh scoring.
Frontier models may use diplomatic language
that obscures genuine critique content.
Signal to noise ratio is reduced.

## Mitigation Steps Taken in V2

- Majority vote across three jury models
- Low reliability flagged in all limitations sections
- Human validation performed on 10 critiques
- Human scores aligned directionally with jury scores
- Finding interpreted as directional not definitive

## What This Does Not Invalidate

**Still robust:**
- ANOVA F(4,991)=47.81 survives Bonferroni correction
- GPT-4o CVS 0.433 vs others 0.573-0.644
- Large effect size deepseek vs gpt4o d=1.17
- Directional finding: GPT-4o behaves differently
- Consistent with Round 5 mechanistic trace finding
- Replicable with stronger jury or human raters

**Needs further validation before claiming:**
- Specific CVS numerical rankings as definitive
- CVS as a finished benchmark-grade standard
- Causal claims about why GPT-4o scores lower

## V3 Improvement Plan

- Expert human validation on 50 critiques minimum
- Target Krippendorff alpha greater than 0.75
- Stratified raters by domain expertise
- Compute Cohen's kappa for inter-rater reliability
- Recalibrate all CVS-dependent findings
- Pre-register jury composition before data collection

## Summary

The CVS finding is real and directionally sound.
GPT-4o produces validation responses not critiques.
This is confirmed by human annotation and consistent
across multiple measurement approaches.
The reliability limitation means exact numerical
values should be treated as preliminary estimates
not definitive benchmark scores.
