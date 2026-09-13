# HealthConnect: Week 6 Project Summary (Data Analytics Track)

**Author:** Hillary Emmanuel

## 1. What I planned to accomplish

Deepen and validate the three strongest Week 5 findings, check whether the Week 4/5 decision to exclude demographics and appointment logistics actually holds up under real testing rather than visual inspection, refresh the KPIs with proper confidence checks, and complete a genuine cross-track integration with Data Science, not just a declared one.

## 2. What I completed

Validated all five Week 5 KPIs via bootstrap resampling, producing confidence intervals instead of bare point estimates. Built the Data Science track's entire missing Week 4 through 6 path independently, since no teammate delivered it, using this track's own findings as the model's feature set. Ran a robustness check that actually tested, rather than assumed, whether excluding demographics and appointment logistics was correct. Revised the Week 6 candidate model recommendation after cross-validation overturned an initial single-split result. Updated the confidence level on the distance finding after two independent methods, bootstrap confidence intervals and permutation importance, converged on the same downgrade.

## 3. What I improved from Week 5

Week 5 reported KPIs as single numbers. Week 6 attaches a confidence interval to each one, so KPI 1's 43.5% to 68.8% range is now known to be reliable for brackets 0 through 2, while the "3+" figure is explicitly flagged as too small a sample to trust precisely. Week 5 could describe that concern; Week 6 can measure it.

## 4. What I integrated

The Data Science model, built entirely from this track's own features, was cross-validated and stress-tested with a full feature set that included everything this track had excluded. Both the model comparison and the permutation importance rankings came back into this track's own conclusions, and materially changed the confidence assigned to the distance finding.

## 5. Which track(s) I collaborated with

Data Science, built independently this week since no teammate delivered Week 4/5 work or responded to outreach. Documented transparently in `cross_track_evidence/DS_Track_Catchup_Note.pdf`.

## 6. What was exchanged

Provided the Week 4/5 finding set and a feature-selection table justifying every included and excluded variable. Received cross-validated model comparisons across three model families and two feature sets, plus permutation importance rankings.

## 7. What changed as a result

The distance finding's confidence level was downgraded, and its business recommendation reframed from "roll out" to "pilot." On the Data Science side, the candidate model recommendation changed from Random Forest to a refined Logistic Regression once cross-validation replaced a single train/test split.

## 8. Key findings or development outcomes

Lead time remains the strongest, most validated finding in the project, now confirmed by three independent methods: the Week 5 point estimates, this week's bootstrap confidence intervals, and the Data Science model's permutation importance. The double-risk stacking segment is now load-bearing in a trained model, not just visible in a segment table. Distance is real, but weaker than it was previously presented.

## 9. Major challenges

The real challenge here wasn't technical, it was avoiding the trap of using a second analysis just to re-confirm what the first one already said. The fix was to deliberately design checks that could disagree with the existing findings, a full feature set instead of a restricted one, bootstrap confidence intervals instead of point estimates, rather than checks built to only agree.

## 10. Important decisions

Chose to downgrade the distance finding's confidence rather than present it at the same level as lead time and prior no-shows, even though the underlying point estimates didn't move. The evidence quality changed, not the recommendation itself.

## 11. Remaining issues

Why distance underperforms in the model relative to its own point-estimate gradient is still unresolved. It's flagged for Week 7 rather than explained away. The "3+" prior no-show bracket also remains too small to trust precisely.

## 12. Contribution to the overall HealthConnect project

Moved this track's findings from single-number claims to confidence-rated ones, and closed the Data Science track's missing path so the project has a working, validated no-show model instead of a gap in the multidisciplinary chain.

## 13. Proposed focus for Week 7

Investigate the distance and lead-time redundancy question, pilot the double-risk confirmation-call intervention on a small scale if the project structure allows it, and if the real Data Science or ML Engineering teammates become available, hand off this track's self-built work for independent review rather than treating it as final.
