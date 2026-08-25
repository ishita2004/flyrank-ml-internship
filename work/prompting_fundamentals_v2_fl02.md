# FL-02: Prompting Fundamentals on Real Tasks v2

**Track:** General AI Fluency  
**Phase:** Foundations  
**Author:** Ishita Chaubey  
**Date:** August 26, 2026  

---

## 1. Selected Task from FL-01 Audit

- **Selected Task:** *Target Task 2: Code Refactoring & Data Pipeline Unit Test Generation* (`Delegate to AI with review`)
- **Real-World Problem:** Data transformation pipeline scripts (`01_prepare_features.py`) process 30,000+ rows of search performance metrics. Silent data corruption (e.g., `avg_position = 0` treated as rank zero, missing `word_count` creating category bias, target leakage from `trend_pct`) causes model evaluation failures. Manual test writing is slow; unit test suites must be auto-generated with strict edge-case coverage.

---

## 2. Prompt Iteration Log (6 Versions)

---

### Version 0 — Naive One-Line Baseline

- **Prompt:**  
  `"Write pytest tests for my python data pipeline script."`
- **Output Excerpt:**  
  ```python
  import pytest

  def test_pipeline():
      data = [1, 2, 3]
      assert len(data) == 3
  ```
- **Iteration Notes:**
  - *Prompt Change:* Naive one-line baseline.
  - *Observed Output Difference:* Generic placeholder test with zero relevance to pandas dataframes or search data.
  - *Remaining Weakness:* Doesn't understand Python data structure, pandas schemas, or edge cases.
  - *Next Technique Applied:* **Role Assignment**.

---

### Version 1 — Named Technique: Role Assignment

- **Prompt:**  
  `"Act as a Senior Principal QA Automation Engineer and Staff Python Developer specializing in scikit-learn and pandas pipelines. Write pytest unit tests for a data preparation function that cleans search metrics."`
- **Output Excerpt:**  
  ```python
  import pytest
  import pandas as pd

  def test_clean_search_metrics():
      df = pd.DataFrame({'impressions': [100, 200], 'clicks': [10, 20]})
      result = clean_search_metrics(df)
      assert isinstance(result, pd.DataFrame)
      assert not result.empty
  ```
- **Iteration Notes:**
  - *Prompt Change:* Assigned role of Senior QA Automation Engineer & Staff Python Developer specializing in pandas.
  - *Observed Output Difference:* Output shifted from generic Python lists to `pandas.DataFrame` testing and type checking (`isinstance`, `not result.empty`).
  - *Remaining Weakness:* Still uses imaginary column names (`clicks`, `impressions`) and fails to test real domain edge cases.
  - *Next Technique Applied:* **Context & Motivation**.

---

### Version 2 — Named Technique: Context & Motivation

- **Prompt:**  
  `"Act as a Senior Principal QA Automation Engineer specializing in pandas pipelines. Write pytest unit tests for our feature prep pipeline. Context & Domain Rules: We process search data (30k rows). Schema: content_id, client_id, impressions_90d, days_since_last_update, avg_position, ctr, trend_direction. Domain Gotchas: avg_position = 0 means 'no search data' (not rank 0). CTR values are percentages (0.76 = 0.76%). trend_pct is derived from trend_direction and MUST be excluded from features. Goal: Prevent silent data corruption."`
- **Output Excerpt:**  
  ```python
  import pytest
  import pandas as pd

  def test_avg_position_zero_handling():
      df = pd.DataFrame({
          'content_id': ['c1', 'c2'],
          'client_id': ['cl1', 'cl1'],
          'impressions_90d': [500, 0],
          'days_since_last_update': [180, 10],
          'avg_position': [0, 5.2],
          'ctr': [0.76, 1.2],
          'trend_direction': ['down', 'up']
      })
      result = prepare_features(df)
      # Checks that avg_position == 0 is flagged or masked correctly
      assert result.loc[result['content_id'] == 'c1', 'has_position_data'].values[0] == False
  ```
- **Iteration Notes:**
  - *Prompt Change:* Provided domain context, schema, and specific business rules (`avg_position = 0` handling, CTR scaling, target leakage prevention).
  - *Observed Output Difference:* Replaced generic checks with domain-specific assertion logic matching our actual dataset schema.
  - *Remaining Weakness:* Generates only one static test case; fails to use parameterized fixtures for multi-value edge testing.
  - *Next Technique Applied:* **Few-Shot Examples**.

---

### Version 3 — Named Technique: Few-Shot Examples

- **Prompt:**  
  `"Act as a Senior QA Automation Engineer. Write pytest tests for our pandas search pipeline using the schema: content_id, impressions_90d, avg_position, ctr, trend_direction. Domain Gotcha: avg_position = 0 means no search data. Here is an example of what good test code looks like in our codebase: \n\nExample:\n@pytest.mark.parametrize('pos_input, expected_flag', [(0, False), (1.5, True), (12.0, True)])\ndef test_position_flag(pos_input, expected_flag):\n    df = pd.DataFrame({'content_id': ['c1'], 'avg_position': [pos_input]})\n    res = add_position_flags(df)\n    assert res['has_position_data'].iloc[0] == expected_flag\n\nNow write complete test suite for zero impressions, nan CTR, and trend_direction mapping."`
- **Output Excerpt:**  
  ```python
  import pytest
  import pandas as pd

  @pytest.mark.parametrize("ctr_input, expected_valid", [(0.76, True), (np.nan, False), (0.0, True)])
  def test_ctr_validity(ctr_input, expected_valid):
      df = pd.DataFrame({'content_id': ['c1'], 'ctr': [ctr_input]})
      res = clean_ctr(df)
      assert (res['ctr'].notna().iloc[0]) == expected_valid
  ```
- **Iteration Notes:**
  - *Prompt Change:* Included a concrete few-shot example demonstrating `@pytest.mark.parametrize` structure.
  - *Observed Output Difference:* Output immediately adopted parameterized test design pattern across all generated test cases.
  - *Remaining Weakness:* Unstructured output; code and text explanations are mixed together unpredictably.
  - *Next Technique Applied:* **Output Structure**.

---

### Version 4 — Named Technique: Output Structure

- **Prompt:**  
  `"Act as a Senior QA Automation Engineer. Write pytest tests for our pandas pipeline. Context: schema (content_id, client_id, impressions_90d, avg_position, ctr, trend_direction). Few-shot style: use @pytest.mark.parametrize. Format your response strictly in XML sections:\n<summary>Brief overview of test cases</summary>\n<fixtures>Pytest fixture code</fixtures>\n<tests>Executable pytest code only</tests>\n<edge_cases>Bullet list of covered edge cases</edge_cases>"`
- **Output Excerpt:**  
  ```xml
  <summary>Coverage includes zero-impression filtering, zero-position masking, and CTR bounds.</summary>
  <fixtures>
  @pytest.fixture
  def sample_search_df():
      return pd.DataFrame(...)
  </fixtures>
  <tests>
  @pytest.mark.parametrize(...)
  def test_prepare_features(sample_search_df):
      ...
  </tests>
  <edge_cases>
  - Zero impressions
  - avg_position == 0
  - NaN handling
  </edge_cases>
  ```
- **Iteration Notes:**
  - *Prompt Change:* Mandated strict XML block output tags (`<summary>`, `<fixtures>`, `<tests>`, `<edge_cases>`).
  - *Observed Output Difference:* Output separated cleanly into isolated code blocks and documentation without conversational chatter.
  - *Remaining Weakness:* Can occasionally jump straight to code without verifying edge-case completeness first.
  - *Next Technique Applied:* **Step Decomposition (Chain-of-Thought)**.

---

### Version 5 — Named Technique: Step Decomposition (Chain-of-Thought)

- **Prompt Version 5 (Final Iterated Prompt):**  
  `"Act as a Senior QA Automation Engineer specializing in pandas search data pipelines. \n\nContext: Input CSV schema is (content_id, client_id, impressions_90d, days_since_last_update, avg_position, ctr, trend_direction). Domain rules: avg_position = 0 means no position data; ctr is percentage (0.76 = 0.76%); trend_pct leaks target trend_direction.\n\nExecute the following steps in order before writing code:\nStep 1: Identify 4 critical edge cases in search performance data.\nStep 2: Define parameterized fixture inputs testing valid, zero, nan, and out-of-bounds values.\nStep 3: Construct clean pytest test suite using @pytest.mark.parametrize.\n\nFormat response strictly in XML tags: <reasoning_steps>, <fixtures>, <test_suite>, <verification_checklist>."`
- **Output Excerpt:**  
  ```xml
  <reasoning_steps>
  Step 1 Edge Cases Identified: 1) avg_position == 0, 2) impressions_90d == 0, 3) missing CTR, 4) trend_pct leakage.
  Step 2 Parameterized Matrix: Validated 4 input vectors across fixtures.
  </reasoning_steps>
  <test_suite>
  import pytest
  import pandas as pd
  import numpy as np

  @pytest.mark.parametrize("imp, pos, expected_keep", [
      (500, 4.2, True),   # Normal visible page
      (0, 5.0, False),    # Zero impressions filtered
      (250, 0.0, True),   # Zero position masked, row kept
  ])
  def test_feature_prep_edge_cases(imp, pos, expected_keep):
      ...
  </test_suite>
  ```
- **Iteration Notes:**
  - *Prompt Change:* Added explicit step decomposition (Step 1 reasoning -> Step 2 fixture matrix -> Step 3 code generation).
  - *Observed Output Difference:* Forced LLM to analyze edge cases *before* writing code, preventing missed boundary conditions.
  - *Remaining Weakness:* None. Production-ready test generator.

---

## 3. Honest Cross-Model Comparison (Claude vs ChatGPT)

Both models were executed using the exact final Version 5 prompt:

| Dimension | Claude (Claude 3.5 / 3.6) | ChatGPT (GPT-4o) |
|---|---|---|
| **Tone & Style** | Exceptionally clean, concise, adhered strictly to XML tags without conversational chatter. | Included conversational intro/outro text outside requested XML tags. |
| **Domain Accuracy** | Handled `avg_position = 0` gotcha perfectly; created explicit boolean mask `has_position_data`. | Correctly handled position data but initially missed testing CTR percentage scaling. |
| **Code Quality** | Clean type annotations, used concise pandas `.loc[]` syntax, no redundant imports. | Used slightly verbose loops inside test functions; added unnecessary helper functions. |
| **Failure Points** | Required explicit instruction to include imports (`import numpy as np`) inside `<test_suite>`. | Tended to hallucinate extra columns (`bounce_rate`, `conversions`) unless strictly constrained. |

---

## 4. Final Reusable Prompt Template (Context-Free)

The following template can be used by any developer on any dataset schema:

```text
Act as a Senior QA Automation Engineer specializing in [DATA_FRAMEWORK, e.g. pandas/polars/PySpark] pipelines.

Context: 
- Function under test: [FUNCTION_NAME]
- Input Schema: [COLUMN_NAMES_AND_TYPES]
- Domain Rules & Constraints: [BUSINESS_RULES_AND_GOTCHAS]

Execute the following steps in order before writing code:
Step 1: Identify 4 critical edge cases (e.g., zero values, NaNs, schema mismatches, out-of-bound inputs).
Step 2: Construct a parameterized fixture matrix covering valid, boundary, and corrupt inputs.
Step 3: Write executable unit tests using [TEST_FRAMEWORK, e.g. pytest].

Format your response strictly using XML tags:
<reasoning_steps>Step-by-step edge case analysis</reasoning_steps>
<fixtures>Reusable test fixtures</fixtures>
<test_suite>Executable test code with @pytest.mark.parametrize</test_suite>
<verification_checklist>Coverage summary checklist</verification_checklist>
```
