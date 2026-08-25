# Three Roads: Choose Your Stack with AI (Week 4)

**Track:** General AI Fluency  
**Phase:** Build  
**Author:** Ishita Chaubey  
**Date:** August 26, 2026  

---

## 1. Input Constraints

To evaluate technical stacks with AI, we provided four explicit operational constraints:

1. **Free Only:** $0 hosting or subscription budget (must use free-tier hosting with SSL).
2. **Honest Skill Level:** Proficient in Python, Pandas, Machine Learning, HTML5/CSS3, and Git; zero interest in managing complex JS build tools or database migrations for a static site.
3. **Portfolio Function Requirements:** Must host a 30-second landing hero, 2 deep-dive case studies, a data contract summary, and a deployed research paper.
4. **Work Display Requirements:** Must render responsive data tables, `matplotlib` SVG feature importance charts, code blocks, LaTeX equations, and direct GitHub links. **No backend required yet.**

---

## 2. The Three Stack Options Evaluated

```text
┌────────────────────────────────────────────────────────────────────────┐
│ OPTION 1: SIMPLEST (Notion + Fruition / Framer Free)                   │
├────────────────────────────────────────────────────────────────────────┤
│ • Build Method: Drag-and-drop or Notion workspace export.              │
│ • Hosting: Framer Subdomain / Netlify (Free).                          │
│ • Backend Required: No.                                                │
│ • Real Trade-off: Fast setup, but terrible for custom responsive tables,│
│   SVG charts, and custom CSS styling. Feels generic and template-bound. │
└────────────────────────────────────────────────────────────────────────┘

┌────────────────────────────────────────────────────────────────────────┐
│ OPTION 2: CHOSEN (Semantic HTML5 / CSS3 + GitHub Pages)                │
├────────────────────────────────────────────────────────────────────────┤
│ • Build Method: Hand-crafted semantic HTML5 & quiet CSS custom props.   │
│ • Hosting: GitHub Pages (Free, native repo integration from /docs).    │
│ • Backend Required: No (100% static delivery).                         │
│ • Real Trade-off: Requires editing HTML tags directly, but offers 100% │
│   control over typography, zero build errors, and perfect SVG rendering.│
└────────────────────────────────────────────────────────────────────────┘

┌────────────────────────────────────────────────────────────────────────┐
│ OPTION 3: MOST POWERFUL (Next.js / React / Tailwind + Vercel)          │
├────────────────────────────────────────────────────────────────────────┤
│ • Build Method: Next.js App Router, TypeScript, Tailwind CSS.          │
│ • Hosting: Vercel Free Tier.                                           │
│ • Backend Required: Optional Serverless Functions + Supabase DB.       │
│ • Real Trade-off: High build-system complexity, npm security alerts,    │
│   and massive over-engineering for a static 4-page research portfolio.  │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Pressure-Testing & Trade-off Analysis

### Pressure-Testing Option 2 (Front-Runner: HTML5 / GitHub Pages)
- *What breaks if I pick the simplest (Option 1)?*  
  Notion / Framer breaks custom responsive table layouts and distorts SVG feature importance charts, making data science work look amateurish.
- *What do I maintain if I pick the most powerful (Option 3)?*  
  Option 3 forces me to maintain `node_modules`, fix breaking Next.js API updates, and troubleshoot Vercel build failures rather than focusing on ML research.
- *Can I finish in two weeks?*  
  Yes. HTML5/CSS3 requires zero build pipeline setup—writing a page takes minutes.
- *Does it show my work the way it needs to be shown?*  
  Yes. Clean HTML/CSS renders research papers, Precision@50 tables, and notebook outputs perfectly.

---

## 4. Final Written Rationale & Decision

> **Chosen Stack:** **Option 2 — Semantic HTML5 / CSS3 hosted on GitHub Pages (`/docs`)**

### Rationale in My Own Words
I chose **Option 2 (HTML5 / CSS3 / GitHub Pages)** because my portfolio's primary job is to present complex machine learning evaluation tables, dataset schemas, and feature importance charts cleanly to senior data scientists.

- **Can I maintain this?** Absolutely. There are no npm packages to update, no build scripts that break, and no serverless functions to debug. The site lives directly in my `ishita2004/flyrank-ml-internship` repository in the `/docs` directory and deploys automatically on every `git push`.
- **Does it display my work well?** Yes. Hand-crafted CSS gives me pixel-perfect control over font hierarchy (`Inter` / `Geist`), table contrast, and responsive SVG chart rendering without clutter.
- **Honest Backend Decision:** **No backend needed.** My target audience wants to evaluate research rigor and code quality, not interact with a dynamic comment box. A static site with direct email links serves 100% of my operational goals.
