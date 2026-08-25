# FL-05: Agent Concepts and MCP Basics

**Track:** General AI Fluency  
**Phase:** Build (core)  
**Author:** Ishita Chaubey  
**Date:** August 26, 2026  

---

## Executive Summary & Explainer (750 Words)

### 1. Workflows vs. Agents: The Fundamental Distinction

In modern AI system design, the word "agent" is frequently misapplied to describe simple prompt chains. To evaluate AI products objectively, one must distinguish between **Workflows** and **Agents** based on control flow governance:

- **Workflows** are systems where the execution sequence, step transitions, and tool invocations are hardcoded by human developers. The LLM performs step-specific transformations (e.g., summarizing or formatting), but it does *not* decide which step comes next. Examples include prompt chains, routing trees, and parallel fan-out pipelines.
- **Agents** are autonomous systems where an LLM dynamically controls its own execution path. Given a high-level objective, the model evaluates intermediate outputs from its environment, decides which tools to invoke, determines parameters, and decides whether to repeat a step, try an alternative strategy, or terminate execution.

#### Classification of the FL-04 Pipeline
Our FL-04 research synthesis pipeline is strictly a **Workflow** (specifically, an Orchestrated Prompt Chain). The pipeline follows a rigid 4-step sequence: `Gather & Extract` $\rightarrow$ `Synthesize & Draft` $\rightarrow$ `Critique & Pressure-Test` $\rightarrow$ `Revise & Format`. The control flow is hardcoded; the LLM cannot skip Step 3 or invent a Step 5 based on runtime feedback.

---

### 2. Model Context Protocol (MCP): The USB-C for AI

The **Model Context Protocol (MCP)** is an open architecture developed by Anthropic that standardizes how LLM applications interface with external data sources, local filesystems, and execution environments. Prior to MCP, every AI integration required bespoke API wrappers. MCP acts as a universal interface, establishing three core primitives:

1. **Tools:** Executable functions exposed by an MCP server that allow the model to take actions in the external world (e.g., `view_file`, `run_command`, `git_commit`). The LLM issues a structured JSON call, the server executes it, and returns the result.
2. **Resources:** Passive data objects provided by an MCP server to supply context (e.g., file contents, database tables, or API documentation).
3. **Prompts:** Pre-configured system prompts and user templates served directly by the MCP server to standardize task execution.

---

### 3. Concrete Agent Upgrade for FL-04

To upgrade our FL-04 static workflow into a true **Autonomous Research Agent**:

- **Current State (Workflow):** Human triggers Step 1 $\rightarrow$ Step 2 $\rightarrow$ Step 3 $\rightarrow$ Step 4 sequentially.
- **Agent Upgrade Architecture:**
  - Give the model an autonomous goal: *"Audit repository scripts for target leakage, run holdout evaluations, and optimize model Precision@50 to exceed 0.70."*
  - Give the model MCP tools (`view_file`, `grep_search`, `run_command`).
  - **Dynamic Loop:** The agent uses `grep_search` to find `trend_pct` references. It edits the script, runs `python scripts/03_train_model.py` via `run_command`, and inspects the resulting `Precision@50` output. If score $< 0.70$, it autonomously chooses to adjust tree hyperparameters, re-runs evaluation, and only stops when the target metric is achieved.

---

## 3. Evidence of Working MCP Setup (Three Demonstrated Tasks)

The following tool-use tasks demonstrate working Model Context Protocol (MCP) tools operating within our codebase, performing actions plain chat cannot execute:

### Task 1: Filesystem Inspection Tool (`view_file`)
- **Tool Called:** `view_file`
- **Target Path:** `d:\Projects\flyrank-ml-internship-starter\data\raw\content_refresh_anonymized.csv`
- **Tool Output:** Read local raw CSV header lines, verifying exact column names (`content_id`, `client_id`, `impressions_90d`, `days_since_last_update`, `avg_position`, `ctr`, `trend_direction`).
- **Why Chat Alone Couldn't Do This:** Plain chat has no access to local disk files without explicit upload.

### Task 2: Codebase Grep Search Tool (`grep_search`)
- **Tool Called:** `grep_search`
- **Query:** `trend_pct` across `SearchPath: d:\Projects\flyrank-ml-internship-starter`
- **Tool Output:** Identified all occurrences of `trend_pct` across python scripts and notebooks to verify target leakage safeguards.
- **Why Chat Alone Couldn't Do This:** Plain chat cannot perform indexed regex searches across multi-directory local codebases.

### Task 3: Local Terminal Shell Execution Tool (`run_command`)
- **Tool Called:** `run_command`
- **CommandLine:** `jupyter nbconvert --to notebook --execute --inplace work/notebooks/w04_baseline_score.ipynb`
- **Tool Output:** Executed Jupyter kernel locally on Windows console, executed pandas code cells, and wrote `work/outputs/baseline_action_score.csv` to disk.
- **Why Chat Alone Couldn't Do This:** Plain chat cannot execute code locally on a user's machine or modify local files directly.
