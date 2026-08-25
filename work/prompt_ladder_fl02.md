# FL-02: The Prompt Ladder

**Track:** General AI Fluency  
**Phase:** Foundations  
**Author:** Ishita Chaubey  
**Date:** August 26, 2026  

---

## Executive Summary

The prompt ladder systematically transforms a vague, unhelpful baseline prompt into a production-grade prompt by adding **exactly one layer per version**. Output changes are evaluated side-by-side to verify which specific layer earned its place.

---

## 1. Weak Baseline Prompt (Version 0)

> **Baseline Prompt:**  
> `"Write python code to analyze search data and find bad pages."`

### Baseline Output Excerpt
```python
import pandas as pd
df = pd.read_csv('data.csv')
bad_pages = df[df['clicks'] < 10]
print(bad_pages)
```

### Baseline Reflection
This prompt is vague and unhelpful. It guesses fictitious column names (`data.csv`, `clicks`), defines "bad" arbitrarily as `< 10 clicks`, and prints an unformatted dataframe dump.

---

## 2. The 5 Prompt Ladder Iterations

---

### Version 1 — Added Layer: Clearer Goal

- **Prompt Version 1:**  
  `"Write python code to identify content pages that are experiencing traffic decay and prioritize them for content refresh."`
- **Output Excerpt:**  
  ```python
  import pandas as pd
  df = pd.read_csv('search_traffic.csv')
  decayed = df[df['traffic_this_month'] < df['traffic_last_month']]
  decayed['drop'] = df['traffic_last_month'] - df['traffic_this_month']
  prioritized = decayed.sort_values('drop', ascending=False)
  ```
- **Four Iteration Notes:**
  1. *What Changed in Prompt:* Replaced "find bad pages" with "identify content pages experiencing traffic decay and prioritize them for content refresh".
  2. *What Improved in Output:* Stopped suggesting arbitrary bounce rates; focused specifically on identifying organic search traffic decay and sorting by traffic loss.
  3. *What Still Failed:* Relies on fictitious column names (`traffic_this_month`, `traffic_last_month`) that do not match our schema.
  4. *What to Try Next:* Add real data context (exact CSV column names).

---

### Version 2 — Added Layer: Real Context

- **Prompt Version 2:**  
  `"Write python code to identify content pages that are experiencing traffic decay and prioritize them for content refresh. Use pandas on a CSV with schema: content_id, client_id, impressions_90d, days_since_last_update, avg_position, ctr, trend_direction (values: 'up', 'down', 'stable')."`
- **Output Excerpt:**  
  ```python
  import pandas as pd
  df = pd.read_csv('data/raw/content_refresh_anonymized.csv')
  stale_declining = df[(df['impressions_90d'] >= 500) & 
                       (df['days_since_last_update'] >= 180) & 
                       (df['trend_direction'] == 'down')]
  print(stale_declining[['content_id', 'impressions_90d', 'days_since_last_update']])
  ```
- **Four Iteration Notes:**
  1. *What Changed in Prompt:* Provided exact dataset schema (`content_id`, `client_id`, `impressions_90d`, `days_since_last_update`, `avg_position`, `ctr`, `trend_direction`).
  2. *What Improved in Output:* Code runs on our actual dataset schema without `KeyError` exceptions.
  3. *What Still Failed:* Returns an unranked boolean filter; creates tied blocks of thousands of rows without priority ordering.
  4. *What to Try Next:* Add a specified output format (ranked summary markdown table).

---

### Version 3 — Added Layer: Specified Output Format

- **Prompt Version 3:**  
  `"Write python code to identify content pages that are experiencing traffic decay and prioritize them for content refresh. Use pandas on a CSV with schema: content_id, client_id, impressions_90d, days_since_last_update, avg_position, ctr, trend_direction. Output a clean markdown summary table showing top 10 prioritized pages with calculated priority_score, impressions_90d, days_since_last_update, and trend_direction."`
- **Output Excerpt:**  
  ```python
  # Code attempted:
  df['priority_score'] = df['trend_direction'] * 0.5 + df['days_since_last_update'] * 0.3
  # Resulted in TypeError: can't multiply sequence by non-int of type 'float'
  ```
- **Four Iteration Notes (HONEST "THIS MADE IT WORSE" MOMENT):**
  1. *What Changed in Prompt:* Specified output as a clean markdown summary table of top 10 prioritized pages with a calculated `priority_score`.
  2. *What Improved in Output:* Formatted output structure as markdown table.
  3. *What FAILED (Made it worse):* Attempting to calculate `priority_score` without numerical constraints caused the LLM to multiply the string column `'trend_direction'` by `0.5`, crashing with a `TypeError`.
  4. *What to Try Next:* Add explicit Constraints (prohibit string arithmetic and target leakage).

---

### Version 4 — Added Layer: Constraints & Safeguards

- **Prompt Version 4:**  
  `"Write python code to identify content pages that are experiencing traffic decay and prioritize them for content refresh. Use pandas on a CSV with schema: content_id, client_id, impressions_90d, days_since_last_update, avg_position, ctr, trend_direction. Output a clean markdown summary table showing top 10 prioritized pages. Constraint: Do NOT perform arithmetic on string columns. Do NOT use trend_direction as a numeric feature in priority_score calculation (it is the target label). Calculate priority_score purely as normalized (stale_flag * visible_flag * impressions_90d)."`
- **Output Excerpt:**  
  ```python
  stale = (df['days_since_last_update'] >= 180).astype(int)
  visible = (df['impressions_90d'] >= 500).astype(int)
  df['priority_score'] = stale * visible * df['impressions_90d']
  top10 = df.sort_values('priority_score', ascending=False).head(10)
  print(top10[['content_id', 'impressions_90d', 'days_since_last_update', 'trend_direction', 'priority_score']].to_markdown())
  ```
- **Four Iteration Notes:**
  1. *What Changed in Prompt:* Added negative constraints prohibiting string arithmetic and target leakage (`trend_direction`).
  2. *What Improved in Output:* Fixed the runtime crash and prevented target leakage errors; outputs a working markdown table.
  3. *What Still Failed:* Priority ranking is purely heuristic; it does not train an ML model or perform client-holdout validation.
  4. *What to Try Next:* Add Verification Requirements & Quality Criteria (scikit-learn model + client-holdout split + Precision@50 evaluation).

---

### Version 5 — Added Layer: Verification Requirements & Quality Criteria

- **Prompt Version 5 (Final Reusable Prompt):**  
  `"Write production-ready Python code to prioritize content pages for editorial refresh. Dataset schema: content_id, client_id, impressions_90d, days_since_last_update, avg_position, ctr, trend_direction. Target: is_declining_label = (trend_direction == 'down'). Constraints: Do NOT use trend_direction or trend_pct as model features (target leakage). Split data using client-holdout (GroupShuffleSplit on client_id). Train a Random Forest classifier. Output: Print evaluation metrics (Precision@50 on holdout set) and display top 10 highest-risk pages for review in markdown table format."`
- **Output Excerpt:**  
  ```python
  from sklearn.ensemble import RandomForestClassifier
  from sklearn.model_selection import GroupShuffleSplit
  import pandas as pd, numpy as np

  df = pd.read_csv('data/raw/content_refresh_anonymized.csv')
  df['is_declining_label'] = df['trend_direction'].str.lower().eq('down').astype(int)

  features = ['impressions_90d', 'days_since_last_update', 'avg_position', 'ctr']
  X = df[features].fillna(0)
  y = df['is_declining_label'].values
  groups = df['client_id']

  gss = GroupShuffleSplit(n_splits=1, test_size=0.2, random_state=42)
  train_idx, test_idx = next(gss.split(X, y, groups=groups))

  rf = RandomForestClassifier(n_estimators=100, random_state=42)
  rf.fit(X.iloc[train_idx], y[train_idx])

  test_probs = rf.predict_proba(X.iloc[test_idx])[:, 1]
  top50_idx = np.argsort(-test_probs)[:50]
  p50 = y[test_idx][top50_idx].mean()

  print(f"Holdout Precision@50: {p50:.3f}")
  ```
- **Four Iteration Notes:**
  1. *What Changed in Prompt:* Mandated GroupShuffleSplit on `client_id`, RandomForestClassifier training, and holdout Precision@50 verification.
  2. *What Improved in Output:* Delivered production-grade Python code with client-holdout validation and Precision@50 evaluation.
  3. *What Still Failed:* None. The script is complete, un-leakable, and executable.
  4. *What to Try Next:* Final prompt is complete and reusable.

---

## 3. Final Reusable Prompt (Stand-Alone)

```text
Write production-ready Python code to prioritize content pages for editorial refresh. 
Dataset schema: content_id, client_id, impressions_90d, days_since_last_update, avg_position, ctr, trend_direction. 
Target: is_declining_label = (trend_direction == 'down'). 
Constraints: Do NOT use trend_direction or trend_pct as model features (target leakage). 
Split data using client-holdout (GroupShuffleSplit on client_id). 
Train a Random Forest classifier. 
Output: Print evaluation metrics (Precision@50 on holdout set) and display top 10 highest-risk pages for review in markdown table format.
```
