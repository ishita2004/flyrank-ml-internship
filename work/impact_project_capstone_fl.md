# General AI Fluency: Impact Project & Portfolio Capstone

**Track:** General AI Fluency  
**Type:** Capstone / Impact Project  
**Author:** Ishita Chaubey  
**Date:** August 26, 2026  

---

## Executive Summary

This deliverable establishes the platform maintenance strategy for keeping my Applied Search Intelligence portfolio continuously updated. It details the process for adding future case studies, names the next real engineering piece, documents the calendar reminder evidence, and preserves the AI build workspace context.

---

## 1. Concrete "How to Add the Next Case" Workflow

Adding a new case study to the portfolio takes under 15 minutes using the established 3-beat structure:

### The 3-Beat Case Structure
1. **The Problem:** 1–2 sentences on what real operational issue or search performance question triggered the build.
2. **What I Did & Decided:** 2–3 sentences on the data contract, model/pipeline choices, and explicit leakage safeguards.
3. **What Came of It:** 1–2 sentences with an honest metric (e.g. Precision@50 improvement, latency reduction, or key data insight) + 1 line on what I'd do differently next time.

### Step-by-Step Execution Steps
1. **Step 1:** Open the preserved Claude Project (*Ishita Chaubey - Portfolio & Search Intelligence Proof*).
2. **Step 2:** Paste raw project notes/metrics and prompt:  
   `"Using my voice card, interview me for 3 questions to extract the 3 beats of this new project: [Project Name]. Then draft the markdown case study."`
3. **Step 3:** Append the generated markdown section into `/work` in [`work/framed_cases_w02.md`](file:///d:/Projects/flyrank-ml-internship-starter/work/framed_cases_w02.md) and rebuild the static site.
4. **Step 4:** Push to GitHub to deploy automatically.

---

## 2. Next Real Piece of Work & Calendar Reminder Evidence

- **Named Next Piece of Work:**  
  **"Full-Warehouse DuckDB & Hugging Face Scaled Opportunity Engine"**  
  *Scope:* Expanding the local 30k-row starter model to query the 78.8M-row Hugging Face warehouse release via DuckDB SQL, partitioning by client history depth and evaluating a 30-day future outcome window.
- **Concrete Calendar Reminder Evidence:**  
  - *Reminder Tool:* Google Calendar & Task Notification.  
  - *Scheduled Date:* **September 23, 2026 (4 weeks out)**.  
  - *Notification Text:* *"Audit & Publish Case Study 3: DuckDB Warehouse Opportunity Engine to Portfolio."*

---

## 3. Preserved Claude Project Build Context (Standing Workspace)

The build context is permanently configured in my **Claude Project Custom System Instructions**, enabling future updates without rebuilding context:

```markdown
# Role & Standing Instructions
You are an expert AI Systems Tutor and Senior ML Architect assisting Ishita Chaubey.

# Voice Card (Mandatory)
Direct, warm, plain, specific, no buzzwords. Short sentences.
Signature words: honest, grounded, proof-first.

# Core Identity Kit & Claim
Ishita builds Applied Search Intelligence and ML prioritizers that convert noisy search data into evidence-backed editorial queues. She proves this to ML Engineering Hiring Managers.

# Platform Maintenance Protocol
When asked to draft a new case study, enforce the 3-beat structure (The Problem, What I Did & Decided, What Came of It) and demand honest metrics (e.g. holdout Precision@K) with zero fluff.
```

---

## 4. Honest Build-in-Public Launch Story

> **Title:** *How I Built an Applied Search Intelligence Portfolio with AI (And What Broke Along the Way)*

When I started building my search intelligence portfolio, I wanted to prove that I could take messy search data and turn it into actionable editorial recommendations. Working with AI transformed how I built, but it required strict boundaries.

- **The Real Win (What AI Did That I Couldn't Alone):**  
  AI acted as a rapid pair programmer, generating parameterized pytest test matrices, automating feature vector transformations, and drafting clean markdown components. It eliminated syntax friction so I could focus on high-level architecture, client-holdout validation, and model selection.
- **The Honest Limitation (What Broke & What I Learned):**  
  Early unconstrained prompts led to severe target leakage—AI happily used `trend_pct` as a numeric feature to achieve an "artificially perfect" 1.000 score. I had to step in, enforce strict negative constraints, bar `trend_pct` from model features, and mandate `GroupShuffleSplit` on `client_id`. This dropped the score to an honest **Precision@50 of 0.740**, but made the model genuinely useful on unseen clients.
