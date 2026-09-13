# HealthConnect: Week 5 to Week 6 Transition (Data Analytics Track)

**Author:** Hillary Emmanuel

Brief transition section per the Week 6 brief, Section 3. Not a repeat of the Week 5 report.

## 1. Main Week 5 output

The Week 5 Analytics package: an EDA and KPI notebook, a Power BI dashboard, and three reports (Week 4→5 Transition, Cross-Track Collaboration, Project Summary).

## 2. Most important result/component produced

The confounding check. It confirmed `booking_lead_days`, `previous_no_shows`, and `distance_to_clinic_km` are statistically independent of each other, but that lead time and prior no-shows stack: patients high on both reach roughly 73% no-show risk, well above either factor alone.

## 3. Main issue or limitation discovered

Two this week.

Reminders show only a weak effect on attendance, weaker than the business scenario implied. That one carries over from Week 5.

The new one: the 15km distance cliff looked real in the raw point estimates but turned out to be far less confident once actually tested. Bootstrap confidence intervals (this week, DA side) and permutation importance from a trained model (DS side) both landed on the same conclusion independently: the middle distance bands can't be told apart statistically. See `KPI_Validation.pdf`, Section 4.

## 4. Track(s) whose work is relevant to Week 6 activities

Data Science. No Data Science teammate delivered Week 4 or Week 5 work, and none responded when contacted, so that track's entire path (problem definition, baseline model, error analysis, improved model, robustness check) got built independently this week, using this track's own Week 4/5 findings as the feature set.

## 5. What is being improved, integrated or validated in Week 6

**Validated:** all five Week 5 KPIs, via bootstrap resampling (`KPI_Validation.pdf`). Three held up fully, one held with an existing caveat now quantified precisely, and one, distance, needed a real confidence downgrade.

**Integrated:** every Data Science modelling decision traces back to a specific Week 4/5 finding, and the model's cross-validated results and permutation importance fed back into this track's own conclusions.

**Improved:** the confidence attached to the distance/15km finding, and the business recommendation built on it, both moved down to match what the evidence actually supports.

---

## Part 1: Integration Readiness

### A. Integration point

**Track worked with:** Data Science.

**Output needed from that track:** an actual predictive model, to test whether the three DA-identified variables, and the interaction/threshold features engineered from them, hold up as real predictors, not just as descriptive point estimates.

**Output provided to that track:** a feature-selection table mapping every DA finding to a specific modelling decision (what to include, what to exclude, and why), plus the underlying processed dataset.

**Why the connection is relevant:** per the Week 6 Integration Matrix, "Data Analytics → Data Science: analytical findings inform modelling decisions."

### B. Integration objective

Before: three variables treated as important based on manual segment-level analysis: box plots, cross-tabs, one confounding check.

After: the same three variables confirmed by a cross-validated classification model as the dominant predictive features, and checked against a version of the model that could see everything DA had excluded, which showed that exclusion was correct rather than assumed.

### C. Evidence

- `cross_track_evidence/Model_Improvement_Validation.pdf` (Data Science track report, copied here for submission), including the Section 7.5 robustness check
- `cross_track_evidence/robustness_cv_results.csv` and `permutation_importance_*.csv` (Data Science track outputs, copied here for submission)
- `KPI_Validation.pdf`, the DA-side bootstrap validation that independently reached the same conclusion on distance as the DS model's permutation importance
