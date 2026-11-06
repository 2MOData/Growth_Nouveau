# Tasko — What really drives growth before the Series A?

An applied data analyst project on a B2B SaaS, built by four friends: **Joe El-Hajj, Colomban Boisseau, Mouad Chouki and Maxime Philippon**.

It was carried out as the defense project of IMT's Growth Modeling & Data-Driven Strategy course (2026). The Tasko case and its fictional dataset come from the course, while the analysis, the model, the report and the slides are our own work.

We started from raw, uncleaned production data and went all the way to recommendations for decision-makers, as a data analyst would. We cleaned and validated the data, built the KPIs, analysed the funnel, the acquisition channels and the customer cohorts, and computed the unit economics. We then built and backtested a forecasting model, sized an A/B test, and presented the results in a written report and a slide deck.

**Stack:** Python (pandas, NumPy, Matplotlib), Jupyter, LaTeX (report and Beamer slides), Git.

## Skills demonstrated

| Area | What we did |
|---|---|
| Data cleaning and quality | Audited 4 raw files; handled duplicates, a test account, mixed date formats, inconsistent labels and sign errors; documented every rule in a cleaning log |
| KPI design | Defined activation, 90-day conversion and churn from the data, and chose a North Star Metric |
| Funnel and cohort analysis | Built the conversion funnel, cohort retention curves and net revenue retention |
| Segmentation and bias control | Compared channels like for like (same company size, same activation status) to rule out composition effects |
| Unit economics | Computed CAC, LTV, LTV : CAC and payback by acquisition channel |
| Change detection | Identified a product change in the time series and measured its impact before and after |
| Forecasting | Built a monthly growth model, backtested it out of sample (MAPE), ran three scenarios and a sensitivity analysis |
| Experimentation | Designed an A/B test with metrics, guardrails, sample size and duration |
| Data visualisation and storytelling | Produced 16 figures, a written report and a 10-slide deck answering a business question |

---

## The question

Tasko is a product-led SaaS for team task management. Everyone can use a free plan, and paid plans are billed per seat (Pro 8 € and Business 12 € per seat and per month). The company has almost no sales team, and paid search is its largest marketing line. The founders believe growth comes from the product itself, but they cannot prove it.

Ahead of a Series A, the board wants to know **what really drives Tasko's growth**. We set out to answer five questions:

1. Where does value leak in the funnel, and is there a real growth loop?
2. Which channels and segments behave differently, and does the difference survive a like-for-like comparison?
3. What are the unit economics (CAC, retention, LTV, payback) by channel and by cohort?
4. What changed over the period, and what did each change cause?
5. Which North Star Metric should the company steer on?

The data covers 18 months of activity (January 2025 – June 2026): 62,311 rows across 4 raw CSV files (workspaces, users, monthly revenue, marketing spend). The company and the data are fictional.

## What we did

**Data cleaning.** The extract came uncleaned from production, with duplicates, an internal test account, mixed date formats, inconsistent labels and impossible values. We wrote each correction as an explicit rule and counted the rows it affected:
- removed an internal account and its 140 users;
- removed 496 duplicate rows;
- harmonised 859 company-size labels;
- converted 956 dates in `DD/MM/YYYY` format;
- corrected 25 negative MRR values, which were sign errors.

The full log is in `figures/cleaning_log.csv` and in the report's appendix.

**Funnel and loop.** Conversion more than doubles once a workspace has at least two teammates within 30 days. We therefore defined activation, the "Aha moment", as a team of three. We then measured the effective virality of the guest loop (`invite_loop`) directly from the data.

**Channels and segments.** We compared activation, conversion, ARPA and churn by channel. We then checked whether the differences hold within the same company size and the same activation status.

**Unit economics.** We computed CAC, revenue LTV (ARPA / churn), LTV : CAC and payback for each paid channel. We also built cohort retention curves and the net revenue retention.

**Change detection.** We detected a lasting step in team invitations from mid-September 2025, most likely an onboarding change. We measured its effect on activation, conversion, the loop and churn, before and after, within channel and size.

**North Star Metric.** We propose the number of workspaces that reach a team of three users each month.

**Growth model.** We built a monthly model that links spend, organic growth and the guest loop to new workspaces, then to conversion (with the observed billing delay), churn and MRR.
- **Backtest:** we calibrated the model on data up to December 2025 and tested it on January–June 2026, including a breakdown of where the error comes from.
- **Scenarios:** we ran three scenarios (pessimistic, base, optimistic) to June 2027, with every parameter taken from values observed in the data.
- **Sensitivity:** we moved each lever by ±20 %, one at a time.

**Experiment design.** We designed an A/B test of an optional "Invite two teammates" step for paid search sign-ups, with metrics, sample size and duration.

## Key findings

- **Growth is product-led after the sign-up.** Activated workspaces convert at 13.8 %, against 5.6 % for the others, and they bring 70 % of all paying customers.
- **The guest loop is real but modest.** Its effective K-factor rose from 0.09 to 0.12. It now brings about one new workspace in five, and it multiplies acquisition rather than replacing it.
- **Paid search buys sign-ups, not teams.** It converts least, even when compared like for like. Its LTV : CAC is 1.16 on revenue, with a payback of 31.5 months. Content SEO is close to 3 : 1 (2.86), and partners destroy value (0.69).
- **The September 2025 onboarding change was the key event.** Activation rose from 38 % to 58 % and conversion from 8.1 % to 10.8 %, with churn unchanged.
- **The outcome is decided after the sign-up.**
  - The model projects a June 2027 MRR of 42 k€, 58 k€ or 109 k€ depending on the scenario.
  - Raising conversion by 20 % adds 14 % of MRR, against only 3.3 % for 20 % more paid search budget.

## Repository structure

```
Growth_Nouveau/
├── data/                         raw CSV files (workspaces, users, monthly_revenue, marketing_spend)
├── figures/                      the 16 figures produced by the notebook + cleaning_log.csv
├── logos/                        images used by the report and the slides
├── latex/
│   ├── rapport/                  written report: rapport.tex, rapportIMT.cls, rapport.pdf
│   └── slides/                   slide deck: slides.tex, slides.pdf
├── Tasko_growth_analysis.ipynb   the full analysis, from cleaning to the experiment plan
└── README.md
```

## How to reproduce

**Notebook.** Run it from the root of the repository, so that the `data/` and `figures/` paths resolve:

```bash
pip install pandas numpy matplotlib jupyter
```

```bash
jupyter notebook Tasko_growth_analysis.ipynb
```

Running all cells regenerates every figure in `figures/`.

**Report and slides.** Compile each document with pdfLaTeX from its own folder, running twice so the table of contents is complete. Both files look for images in `figures/` and `logos/`, either next to the `.tex` file or at the root of the repository. They compile unchanged locally and on Overleaf.

```bash
cd latex/rapport && pdflatex rapport.tex && pdflatex rapport.tex
```

```bash
cd latex/slides && pdflatex slides.tex && pdflatex slides.tex
```
