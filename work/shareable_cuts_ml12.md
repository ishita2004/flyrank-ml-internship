# ML-12: Package Your Story — Demo Outline & Shareable Cuts

**Track:** Machine Learning  
**Phase:** Submit  
**Author:** Ishita Chaubey  
**Date:** August 31, 2026  

---

## 1. 5-Minute Showcase Demo Outline

- **Min 0-1: Frame the Operational Decision**  
  - *Hook:* Content teams manage 30,000+ published URLs but only have bandwidth to review 20 to 50 pages per week.  
  - *Problem:* Traditional rules (e.g. `age >= 180 AND impressions >= 500`) create massive tied blocks and ignore non-linear position/CTR decay.

- **Min 1-2: Data & Leakage Safeguards**  
  - *Data:* 28,795 candidate articles across 32 client domains (`FlyRank/internship-warehouse`).  
  - *Leakage Control:* Barred `trend_pct` and `trend_direction` from inputs. Demonstrated that including `trend_pct` yields an artificially perfect 1.000 score, whereas removing it restores honest validation.

- **Min 2-3: Method & Client-Holdout Results**  
  - *Validation:* Enforced `GroupShuffleSplit` on `client_id` (testing strictly on unseen clients).  
  - *Key Result Chart:* Showed Random Forest model achieving **0.740 Precision@50**, delivering a **3.08x lift** over the hand-written rule baseline (0.240).

- **Min 3-4: The Action Playbook Queue**  
  - Walked through the top-ranked recommendation queue (`refresh_content`, `optimize_snippet`, `rearchitect_links`) with transparent reason codes.

- **Min 4-5: Limitations & Takeaway**  
  - *Framing:* Observational decision support (no causal claims or search engine reverse-engineering).  
  - *Link:* Deployed paper live at `https://github.com/ishita2004/flyrank-ml-internship/blob/main/docs/index.html`.

---

## 2. Shareable Cut 1: Short Social Methodology Post

> *"Can machine learning double the ROI of content refresh audits?*  
>  
> *When managing 30k+ URLs, static rules like 'update articles older than 6 months' create massive queue bottlenecking. In my latest FlyRank ML Internship capstone, I built a client-holdout validated Random Forest prioritizer on 28,795 search performance rows.*  
>  
> *Key insight: By barring target-derived leakage features (`trend_pct`) and evaluating strictly on unseen client domains (`GroupShuffleSplit`), our model achieved a **0.740 Precision@50**—a **3.0x lift** over hand-written heuristics. 37 out of the top 50 flagged pages are confirmed decaying high-traffic assets.*  
>  
> *Read the full open research paper and inspect the code receipts here: https://github.com/ishita2004/flyrank-ml-internship/blob/main/docs/index.html"*

---

## 3. Shareable Cut 2: 3-Sentence Employer-Facing Summary

> *"I build applied search intelligence systems that convert noisy performance data into evidence-backed operational queues. In this capstone, I developed a refresh opportunity model using 28,795 rows of production search analytics across 32 clients, enforcing client-holdout splits and strict leakage controls. The resulting Random Forest model achieved a 0.740 Precision@50 on unseen client domains (3.0x over baseline), providing content teams with a human-reviewed action playbook."*
