# FL-04: Multi-Step No-Code Workflow & Walkthrough

**Track:** General AI Fluency  
**Phase:** Build (core)  
**Author:** Ishita Chaubey  
**Date:** August 26, 2026  

---

## Executive Summary

This deliverable documents an automated multi-step research synthesis pipeline (**Draft, Critique, Revise**) designed to process technical machine learning papers and search analytics research. The workflow chains NotebookLM and Claude Project prompts across 4 distinct steps, cutting research synthesis time by **55.6%** while maintaining strict source grounding.

---

## 1. Workflow Architecture & Step Diagram

```text
┌────────────────────────────────────────────────────────────────────────┐
│ STEP 1: GATHER & EXTRACT (NotebookLM / Source Extraction)              │
│   • Input: Raw Technical Paper / PDF / ArXiv Source Text               │
│   • Action: Extract core research claims, dataset size, & metrics      │
│   • Handoff 1: Structured Claim Matrix (JSON/Markdown)                │
├────────────────────────────────────────────────────────────────────────┤
│ STEP 2: SYNTHESIZE & DRAFT (Claude Project Draft Engine)              │
│   • Input: Structured Claim Matrix from Step 1                         │
│   • Action: Draft 300-word executive summary using Voice Card style    │
│   • Handoff 2: Initial Synthesis Draft (v1)                            │
├────────────────────────────────────────────────────────────────────────┤
│ STEP 3: CRITIQUE & PRESSURE-TEST (ChatGPT / Adversarial Reviewer)      │
│   • Input: Draft v1 + Original Claim Matrix                            │
│   • Action: Audit draft for hallucinated claims & missing metrics      │
│   • Handoff 3: Critique Log & Revision Checklist                       │
├────────────────────────────────────────────────────────────────────────┤
│ STEP 4: REVISE & FORMAT (Claude Final Polisher)                       │
│   • Input: Draft v1 + Critique Log from Step 3                         │
│   • Action: Resolve critique flags, enforce plain voice, format tags   │
│   • Final Output: Publication-Ready Research Summary                   │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Prompts & Configuration Used

### Step 1 Prompt (Gather & Extract)
```text
Act as a Principal Research Analyst. Read the attached document and extract:
1. Primary Research Question
2. Dataset Size & Time Windows
3. Core Methodology & Model Types Evaluated
4. Exact Key Metrics (Baseline vs Model)
5. Top 2 Stated Limitations
Format strictly as XML: <claim_matrix><question/><dataset/><method/><metrics/><limits/></claim_matrix>
```

### Step 2 Prompt (Synthesize & Draft)
```text
Using the <claim_matrix> from Step 1, draft a 300-word research summary. 
Voice Card Rules: Direct, warm, plain, specific, no buzzwords. Short sentences.
Structure: 
- Headline (One sentence)
- Problem & Decision Supported
- Observed Results (Highlight metrics)
- Limitations
Do not add adjectives or un-grounded claims.
```

### Step 3 Prompt (Critique & Pressure-Test)
```text
Act as an Adversarial Peer Reviewer. Compare <draft_v1> against <claim_matrix>.
Check:
1. Did the draft introduce any metric numbers not present in the claim matrix?
2. Did it use buzzwords like "cutting-edge" or "transformative"?
3. Did it over-state causal claims?
Output a bulleted list of required corrections in <critique_log>.
```

### Step 4 Prompt (Revise & Format)
```text
Apply all corrections listed in <critique_log> to <draft_v1>. Output the polished final research summary in clean markdown format.
```

---

## 3. Five Real Documented Runs

### Run 1: FlyRank Search Intelligence March 2026 Technical Whitepaper
- **Input:** `docs/flyrank-seo-research-march-2026.pdf` (30k rows dataset release).
- **Step 1 Extract:** 28,795 candidate rows, 32 clients, 90-day feature window.
- **Step 2 Draft:** Drafted 280-word refresh model summary.
- **Step 3 Critique:** Flagged missing Precision@50 baseline comparison number (0.240 vs 0.740).
- **Step 4 Output:** Final summary produced with complete 3.0x lift metrics.

### Run 2: Decision Tree vs Random Forest Client-Holdout Study
- **Input:** Notebook 02 experiment cell outputs.
- **Step 1 Extract:** GroupShuffleSplit client holdouts, depth=3 vs Random Forest n=100.
- **Step 2 Draft:** Drafted model depth comparison note.
- **Step 3 Critique:** Caught minor typo in depth threshold reporting.
- **Step 4 Output:** Corrected and finalized.

### Run 3: SERP CTR Decay & Position Tier Analysis
- **Input:** Discovery notebook CTR analysis output.
- **Step 1 Extract:** CTR decay across position tiers 1-3, 4-10, 11-20.
- **Step 2 Draft:** Drafted CTR cliff synthesis.
- **Step 3 Critique:** Verified non-linear cliff drops between pos 3 and pos 4.
- **Step 4 Output:** Clean publication summary.

### Run 4: Target Feature Leakage Audit (trend_pct Experiment)
- **Input:** Notebook 03 leakage test logs.
- **Step 1 Extract:** Leaky Precision@50 (1.000) vs Honest Precision@50 (0.540).
- **Step 2 Draft:** Drafted leakage warning report.
- **Step 3 Critique:** Ensure target definition is explicitly named.
- **Step 4 Output:** Finalized.

### Run 5: Editorial Review Queue & Reason Codes Engine
- **Input:** Capstone notebook top 10 queue output.
- **Step 1 Extract:** 10 prioritized content IDs with reason codes (`stale_visible_page`, `model_decline_risk`).
- **Step 2 Draft:** Drafted action playbook.
- **Step 3 Critique:** Verified reason code mapping logic.
- **Step 4 Output:** Complete.

---

## 4. Honest Time-Saved Accounting

| Activity | Manual Time | Workflow Time | Net Savings |
|---|---|---|---|
| Pipeline Setup & Prompt Tuning | 0 mins | 60 mins (one-time) | -60 mins |
| Research Run 1 | 45 mins | 8 mins | +37 mins |
| Research Run 2 | 45 mins | 8 mins | +37 mins |
| Research Run 3 | 45 mins | 8 mins | +37 mins |
| Research Run 4 | 45 mins | 8 mins | +37 mins |
| Research Run 5 | 45 mins | 8 mins | +37 mins |
| **Total Time (5 Runs)** | **225 mins (3.75 hrs)** | **100 mins (1.67 hrs)** | **+125 mins (55.6% Net Savings)** |

---

## 5. Known Failure Points & Required Human Review

1. **Sign & Direction Inversion Risk:** LLMs can occasionally swap negative signs in correlation coefficients (e.g. reporting $r = -0.007$ as $+0.007$). **Human Check Required:** Verify raw statistical outputs in python cells.
2. **Missing Out-of-Sample Context:** If a paper doesn't state its train/test split, the LLM will assume standard random split. **Human Check Required:** Ensure client-holdout or time-aware split is explicitly noted.
