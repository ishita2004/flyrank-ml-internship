# FL-01: AI Workflow Audit and Tool Setup

**Track:** General AI Fluency  
**Phase:** Onboarding & Workflow Audit  
**Author:** Ishita Chaubey  

---

## 1. Weekly Workflow Audit (12 Recurring Tasks)

The following table maps 12 recurring tasks across academic study, machine learning engineering, and personal project management into four operational AI delegation tiers.

| # | Task Description | Domain | AI Classification | One-Line Rationale |
|---|---|---|---|---|
| 1 | **Finalizing Capstone Project Scope & Core Architecture** | ML / Work | **Just me** | Strategic choices require personal ownership, domain intuition, and accountability for alignment with career goals. |
| 2 | **Handling Team Conflict & Peer Feedback Sessions** | Leadership | **Just me** | Interpersonal dynamics require genuine empathy, emotional nuance, and unmediated human connection. |
| 3 | **Synthesizing Academic Papers & Literature Background** | Research | **Collaborate with AI** | AI rapidly extracts key methodologies and findings, while I evaluate claims, rigor, and relevance. |
| 4 | **Brainstorming Feature Engineering Ideas for Datasets** | ML / Data | **Collaborate with AI** | AI generates broad domain signals and interactions, which I filter against physical data constraints. |
| 5 | **Drafting Technical Documentation & API Guides** | Development | **Delegate to AI w/ Review** | AI structures docstrings and markdown layouts, which I review for exactness and technical correctness. |
| 6 | **Refactoring Python Pipeline Scripts & Adding Type Hints** | Code | **Delegate to AI w/ Review** | AI handles repetitive syntax and typing additions, which I verify through unit test execution. |
| 7 | **Writing Unit Tests & Edge-Case Test Suites** | QA / Code | **Delegate to AI w/ Review** | AI generates comprehensive boundary tests, which I execute and audit against failure modes. |
| 8 | **Translating SQL Queries into DuckDB / Pandas Pipelines** | Data | **Delegate to AI w/ Review** | Syntax translation across dialects is mechanical and fast for AI, requiring only output validation. |
| 9 | **Generating Standard Weekly Status Reports & Summaries** | Project Mgmt | **Fully Automate** | Structured logs and git diffs can be parsed automatically into standardized markdown progress summaries. |
| 10 | **Formatting Code according to PEP8 / Black Standards** | Code Quality | **Fully Automate** | Linter/formatter CLI hooks enforce deterministic code style without requiring manual intervention. |
| 11 | **Syncing Local Repository Commits with Remote GitHub** | Operations | **Fully Automate** | Automated script runners handle standard git staging, committing, and pushing workflows smoothly. |
| 12 | **Verifying Environment Requirements & Dependency Locks** | DevOps | **Fully Automate** | Automated package verification scripts check `requirements.txt` environment lock states instantly. |

---

## 2. Tool Accounts & Academy Setup

| Tool / Platform | Status | Verification & Next Step |
|---|---|---|
| **Claude (Anthropic)** | Configured | Active account setup; Claude Project created with custom system prompt (see Section 3). |
| **ChatGPT (OpenAI)** | Configured | Active account setup for multi-model cross-verification and comparison. |
| **Anthropic Academy** | Enrolled | Enrolled in *AI Fluency: Framework & Foundations*; completed Module 1 (*Collaborating with AI Effectively*). |

---

## 3. Custom Instructions for Claude Project

Copy and paste the following prompt into your **Claude Project Custom Instructions** panel:

```markdown
# Role & Context
I am Ishita Chaubey, a Machine Learning Intern and Applied AI Specialist working on search intelligence, data science, and scalable software systems.

# Preferred Communication Tone & Style
- Concise, technical, and direct. Avoid unnecessary fluff or conversational preamble.
- Format responses using clean GitHub-flavored markdown with structured tables, bullet points, and code blocks.
- When suggesting code, write production-grade Python/SQL with clear inline comments and type annotations.
- Never guess or extrapolate data schemas; always base logic strictly on provided schema context.

# Current Objectives & Focus
- Building applied search intelligence models (ranking, CTR optimization, content decay scoring).
- Mastering AI workflow fluency: knowing when to delegate, collaborate, automate, or execute personally.
- Maintaining rigorous validation standards (client-holdout splits, zero-leakage guarantees, Precision@K metrics).
```

---

## 4. Three Target Tasks for FL-02 through FL-04

These three recurring tasks will serve as testbeds for advanced AI prompt engineering, delegation frameworks, and automation scripts.

### Target Task 1: Synthesizing Academic Literature & Research Papers
- **Classification:** `Collaborate with AI`
- **What "Done Well" Means:**
  - Extracts core methodology, baseline models, dataset splits, and key quantitative findings within 5 minutes per paper.
  - Identifies implicit assumptions or limitations noted by the authors.
  - Produces a 3-bullet decision summary answering: *"Can we apply this technique to our dataset?"*

### Target Task 2: Code Refactoring & Data Pipeline Unit Test Generation
- **Classification:** `Delegate to AI with review`
- **What "Done Well" Means:**
  - Achieves >85% line coverage for utility modules and data transformation functions.
  - Identifies edge cases (e.g., zero impressions, missing columns, inf/nan values) without breaking function signatures.
  - Passes 100% of generated pytest tests without manual code modifications.

### Target Task 3: Automated Weekly Progress & Performance Reporting
- **Classification:** `Fully Automate`
- **What "Done Well" Means:**
  - Parses weekly git commits, updated notebook cell metrics, and model results JSON files into a clean `report.md`.
  - Zero manual formatting required; executes automatically via a single command or script hook.
  - Highlights top performance metrics (e.g., Precision@50) and flags outstanding blockers automatically.
