# HealthConnect: Week 6 Mandatory Cross-Track Integration (Data Analytics Track)

**Author:** Hillary Emmanuel

Per the Week 6 brief, Section 9: "Communication alone is not sufficient. The collaboration must result in a meaningful change, improvement, decision, integration or validated output." This documents that exchange.

## 1. Track collaborated with

Data Science.

## 2. Project dependency

Per the Week 6 Integration Matrix: "Data Analytics → Data Science: analytical findings inform modelling decisions." The Data Science track's classification model can't exist without a defined target, justified features, and a documented cancellation-handling decision, all of which come from this track's Week 4/5 output.

## 3. Information/output received

A trained, cross-validated classification model and its outputs: ROC-AUC comparisons across three model families and two feature sets (restricted vs. full), plus permutation importance rankings showing which features actually drive predictive performance, not just which ones show separation in a box plot.

## 4. Information/output provided

The Week 4/5 finding set (lead time, prior no-shows, and distance as the three separating variables; the confounding check showing them independent but stacking; the "Cancelled must stay distinct from No-Show" framing), translated into a feature-selection table with a stated justification for every included and excluded variable (see `cross_track_evidence/Model_Improvement_Validation.pdf`, Section 3).

## 5. Integration activity completed

This went both ways, not a one-off handoff:

1. This track's Week 4/5 findings became the Data Science model's entire feature set, restricted to the variables this track had validated.
2. The Data Science track then tested whether excluding the other variables (demographics, appointment logistics) was actually correct, not just assumed, by training models on a full feature set that included them, under 5-fold cross-validation.
3. The Data Science track's permutation importance results came back to this track and were checked against an independent method run here: bootstrap confidence intervals on each KPI (`KPI_Validation.pdf`).
4. Both methods, run independently on the same question, reached the same conclusion about the distance variable.

## 6. What changed as a result

This track's decision to exclude demographics and appointment logistics from the KPI analysis is now evidence-tested rather than based on visual inspection alone. The distance ("15km cliff") finding's confidence level is downgraded in this track's Week 6 report, directly because of the Data Science permutation-importance result. That's the other track's output actually changing this track's conclusion, not just confirming it. And the Data Science track's own candidate model recommendation changed from Random Forest to a refined Logistic Regression, because cross-validation (run specifically to make this integration a fair comparison) overturned an earlier single-split result.

## 7. Evidence

- `cross_track_evidence/Model_Improvement_Validation.pdf` (Data Science track report, copied here for submission, Sections 3, 7.5, 8, 10)
- `cross_track_evidence/robustness_cv_results.csv` (Data Science track outputs, copied here for submission)
- `cross_track_evidence/permutation_importance_random_forest.csv`, `permutation_importance_gradient_boosting.csv` (Data Science track outputs, copied here for submission)
- `KPI_Validation.pdf` (this track's independent confirmation)
- `cross_track_evidence/DS_Track_Catchup_Note.pdf` (copied here for submission, disclosure of how this collaboration was structured)
