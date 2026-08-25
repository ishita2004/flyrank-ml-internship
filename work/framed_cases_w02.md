# Frame It as Cases: Work That Speaks for Itself (Week 2)

**Track:** General AI Fluency  
**Phase:** Foundations  
**Author:** Ishita Chaubey  
**Date:** August 26, 2026  

---

## 1. Voice Card (Standing Instruction)

> **Voice Card:**  
> *Direct, warm, plain, specific, no buzzwords.*  
> **Signature Words:** *honest, grounded, proof-first.*

### Custom System Instruction Addition for AI Workspace
```markdown
# Voice Card Rules
- Speak directly, plainly, and specifically. Short sentences.
- Never use buzzwords or hype terms like "passionate," "results-driven," "dynamic," "cutting-edge," or "game-changing."
- Ground every claim in concrete observed data or decisions.
- When drafting case studies or bios, write like a real person talking to an engineering peer.
```

---

## 2. Before & After: Generic AI Copy vs Edited Voice

To demonstrate the difference between generic AI-generated copy and a grounded voice:

| Version | Draft Text | Analysis |
|---|---|---|
| **Before (Generic AI)** | *"I am a passionate, results-driven AI Engineer leveraging cutting-edge machine learning algorithms to pioneer transformative data-driven solutions and deliver high-impact business outcomes."* | Vague, adjective-heavy, uses 5 buzzwords, and tells the reader nothing about actual capabilities or past work. |
| **After (Edited Voice)** | *"I build applied search intelligence models that prioritize decaying content pages, testing every model on unseen clients so the numbers are honest."* | Direct, specific, names the actual problem solved, and highlights the validation discipline without fluff. |

---

## 3. Case Studies (Three Beats Format)

### Case Study 1: Content Refresh Opportunity Engine
*Target Audience: Lead Data Scientist / ML Engineering Manager*

- **The Problem:**  
  Large content marketing sites manage thousands of published articles, but editorial teams only have bandwidth to update 20 to 50 pages per week. Stale articles gradually lose organic search position, but human reviewers cannot audit thousands of URLs manually.
- **What I Did & Decided:**  
  I built an end-to-end Python ML pipeline using `scikit-learn` and `pandas` to score and rank content decay risk. To prevent data leakage, I explicitly barred `trend_direction` and `trend_pct` from input features since they encode the target label. I enforced **client-holdout validation** (`GroupShuffleSplit`), testing trained models strictly on clients they had never seen before.
- **What Came of It:**  
  The Random Forest model achieved a **Precision@50 of 0.740** on unseen client holdouts—beating the hand-written rule baseline (**0.240**) by **3.0x**. Of the top 50 pages recommended for review, 37 were actively declining high-demand assets, saving editorial teams hundreds of wasted review hours.
- **What I Would Do Differently Next Time:**  
  Instead of static historical target proxies, I would construct rolling 30-day future outcome windows on the full Hugging Face warehouse release to evaluate long-term recovery retention.

---

### Case Study 2: Search Signal Audit & CTR Cliff Analysis
*Target Audience: Search Intelligence & Content Analytics Leads*

- **The Problem:**  
  Teams frequently assume that target keyword search volume directly dictates page traffic and that writing longer articles is the primary lever for ranking retention.
- **What I Did & Decided:**  
  I conducted a signal audit on 30,000 anonymized search performance rows, computing live statistical correlations between keyword search volume and 90-day impressions, and analyzing Click-Through Rate (CTR) decay across position tiers.
- **What Came of It:**  
  I discovered that keyword search volume has a **near-zero correlation ($\approx 0.007$)** with actual impressions gained by a page, and that median word count between growing ($1,475$ words) and declining ($1,514$ words) pages is virtually identical. This proved that article length is not the ranking lever, steering our modeling efforts toward position-adjusted CTR cliffs and freshness signals instead.
- **What I Would Do Differently Next Time:**  
  I would segment expected CTR by intent classification (`informational` vs `transactional`) to isolate query-level SERP feature disruptions.

---

## 4. Bio & Contact CTA Copy

### Bio Options (Plain & Human)
1. **Selected Bio:**  
   *"I build applied search intelligence models and ML prioritizers that turn noisy search data into evidence-backed editorial queues. I focus on honest validation, leakage control, and clear decision support."*
2. **Alternative:**  
   *"I work at the intersection of search data and machine learning, building readable models that tell content teams which pages to fix first."*

### Contact Prompt / Action CTA
- **Primary CTA:**  
  *"If you're looking for an Applied ML Engineer who tests assumptions against real data before building models, [email me to set up a technical chat](mailto:ishitax2004@gmail.com)."*
