# HealthConnect Clinic — Week 6 (Data Analytics Track)

**Programme:** AnalystLab Africa Experience Lab
**Project:** Improving Patient Appointment Attendance and Healthcare Support Using Data and AI
**Track:** Data Analytics
**Prepared by:** Hillary Emmanuel
**Week 6 focus:** Advanced Analytics & Decision Support (deepen and validate Week 5 findings, not repeat them)

**Google Drive submission folder:** to be added

---

## What's in this repository

| Path | What it is |
|---|---|
| `reports/Part1_Week5_to_Week6_Transition.md` / `.pdf` | Week 5→6 transition and Part 1 Integration Readiness |
| `reports/Advanced_Analytics_Decision_Support.md` / `.pdf` | Main Week 6 deliverable: deepened findings, refined KPIs, business recommendations, limitations, Week 7 requirements |
| `reports/KPI_Validation.md` / `.pdf` | Bootstrap confidence-interval validation of all five Week 5 KPIs |
| `reports/CrossTrack_Collaboration_Week6.md` / `.pdf` | The mandatory Week 6 cross-track integration writeup (Data Analytics ↔ Data Science) |
| `reports/Week6_Project_Summary.md` / `.pdf` | The Week 6 Project Summary |
| `reports/cross_track_evidence/` | Copies of the Data Science track's evidence (model report, cross-validation results, permutation importance) referenced in the cross-track writeup |
| `notebooks/HealthConnect_Week6_KPI_Validation.ipynb` | Executed notebook reproducing the KPI bootstrap validation with real outputs |
| `outputs/dashboard/HealthConnect_Week6_Dashboard.pbix` | Updated Power BI dashboard: bootstrap confidence-interval error bars on all four rate charts, revised title, distance-finding caption |
| `outputs/dashboard/HealthConnect_Week6_Dashboard.pdf` / `.png` | Static exports of the updated dashboard |
| `data/HealthConnect_Appointment_Data.csv` | Original dataset, never modified |
| `data/HealthConnect_Data_Dictionary.xlsx` | Original data dictionary |
| `data/processed/healthconnect_appointments_prepared.csv` | Prepared dataset carried forward from Week 5 |
| `data/processed/kpi1_ci_prior_no_show_bracket.csv`, `kpi2_ci_lead_time_band.csv`, `kpi3_ci_distance_band.csv`, `kpi4_ci_reminder_sent.csv` | Bootstrap CI tables imported into Power BI to drive the dashboard's error bars |

This folder does not include a `scripts/` directory. The KPI validation logic that would normally live there is merged directly into the notebook, since the analysis is being submitted as final documents rather than as a reproducible pipeline this week.

---

## Reproducing the analysis

Developed on **Python 3.14**, using the same environment set up for the Week 6 Data Science track (`../week 6 - data science track/.venv`).

```bash
cd "../week 6 - data science track"
.venv\Scripts\activate          # Windows
cd "../week 6/notebooks"
jupyter notebook
```

Then open `HealthConnect_Week6_KPI_Validation.ipynb` and run all cells top to bottom. It reads the prepared dataset from `../data/processed/` read-only.

To open the Power BI dashboard, open `outputs/dashboard/HealthConnect_Week6_Dashboard.pbix` in Power BI Desktop. It reads the same prepared dataset plus the four `kpi*_ci_*.csv` files for its confidence-interval error bars. The `.pdf` / `.png` files are static exports for anyone without Power BI Desktop.

---

## Headline findings

- **Booking lead time remains the strongest, most validated finding in the project.** Confirmed by three independent methods this week: the Week 5 point estimates, this week's bootstrap confidence intervals, and the Data Science track's cross-validated model, where it ranks as the dominant predictive feature by a wide margin.
- **The double-risk stacking segment** (long lead time and multiple prior no-shows, ~4.7% of bookings, ~73% risk) is now load-bearing in a trained classification model, not just visible in a segment table.
- **The distance "15km cliff" finding is downgraded.** Bootstrap confidence intervals show the three middle distance bands are statistically indistinguishable from each other, independently confirmed by the Data Science track's permutation importance check. Distance-based intervention should now be framed as a pilot hypothesis, not a confirmed lever.
- **The Week 4/5 decision to exclude demographics and appointment logistics from the analysis is now evidence-tested**, not just based on visual inspection, via a robustness check on the Data Science side that added those variables back into a full model and found no benefit.

See `reports/Advanced_Analytics_Decision_Support.pdf` for the full writeup and `reports/KPI_Validation.pdf` for the complete per-KPI validation detail.
