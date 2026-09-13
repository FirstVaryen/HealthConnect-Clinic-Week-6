# HealthConnect: Advanced Analytics & Decision Support (Week 6, Data Analytics Track)

**Author:** Hillary Emmanuel
**Track:** Data Analytics
**Week 6 focus (per brief):** deepen and validate the Week 5 findings, and use them to support project decisions. Not a repeat of the Week 5 EDA or the same dashboard.

---

## 1. Which Week 5 findings needed deeper investigation

Three, chosen because they carry the most weight in Week 5's recommendations and would be the most costly to get wrong:

1. The lead-time relationship. It's the strongest predictor, but is the shape actually a smooth climb, or does the risk concentrate somewhere specific?
2. The double-risk stacking effect (lead time × prior no-shows, roughly 73% combined risk).
3. The distance "cliff" at 15km.

## 2. Deeper investigation and results

### 2.1 Lead-time relationship: confirmed, stronger than before

Bootstrapped confidence intervals (`KPI_Validation.pdf`, Section 3) show a clean, non-overlapping climb across every lead-time band: 27.8% to 33.6% to 43.2% to 53.8% to 67.7%. Separately, the Data Science track's cross-validated model ranks `booking_lead_days` as the dominant predictive feature, five to ten times any other variable (permutation importance 0.097-0.105). Two unrelated methods reach the same conclusion. This is the single most reliable finding in the project.

### 2.2 Double-risk stacking effect: confirmed, and now doing real work in a model

The Week 5 confounding check found lead time and prior no-shows are independent but stack to roughly 73% risk when combined. This week, the Data Science track built that exact interaction (`leadtime_x_priornoshow`) as a model feature, and it came back as the second most important feature in every model tested (permutation importance 0.025-0.029). A trained classifier actually needs it to perform well.

### 2.3 Distance cliff: this is the one that changed

Full detail in `KPI_Validation.pdf`, Section 4. Short version: bootstrapped confidence intervals for the three middle distance bands (under 5km, 5-10km, 10-15km) overlap almost completely, so the data can't reliably tell these groups apart. Only the two extremes separate, and just barely. The Data Science track's permutation importance check, run on a trained model rather than on rate confidence intervals, found the same thing: distance contributes very little predictive value next to lead time and prior no-shows.

That doesn't mean distance is irrelevant. The point estimates still show a real gradient, 46.5% up to 54.1%. It means the evidence behind it is meaningfully weaker than for the other two findings, and it shouldn't be presented, funded, or acted on with the same confidence.

## 3. Segment investigation (patient/appointment/operational)

Week 4/5 concluded that demographics and appointment logistics (age, gender, appointment type, day, time) show no meaningful separation from the outcome. Rather than take that at face value, the Data Science track's robustness check (Section 7.5 of `cross_track_evidence/Model_Improvement_Validation.pdf`) added all of these variables back into a full model and used permutation importance to check directly whether any of them mattered once a model had the chance to use them.

Adding them back made every model slightly worse. Restricted-feature Logistic Regression scored 0.682 ROC-AUC cross-validated, against 0.677 with demographics and logistics added in. A few of them (appointment type follow-up, Wednesday, Thursday, age) technically land in the model's top 10 features, but at importance values 20 to 1,000 times smaller than lead time. The Week 4/5 exclusion decision is now tested, not assumed, which is a stronger basis for the same conclusion than the original visual inspection gave.

## 4. Refined KPIs

Full detail in `KPI_Validation.pdf`. What changed:

| KPI | Status after Week 6 validation |
|---|---|
| 1: Prior no-show bracket | Robust for brackets 0-2; the "3+" bracket (n=93) can't be statistically told apart from bracket "2" |
| 2: Lead-time band | Fully validated, the strongest KPI in the project |
| 3: Distance band | Downgraded, middle bands statistically indistinguishable, weaker than 1, 2, or 5 |
| 4: Wasted slot rate / reminders | Confirmed weak but real, same confidence as Week 5 |
| 5: Cancellation-to-no-show ratio | Validated, the trend is real and stable |

## 5. Findings ranked by business impact

1. **Booking lead time (KPI 2).** The strongest, most validated finding. A 31+ day booking crossing the 50% no-show mark is a clear, actionable threshold.
2. **The double-risk stacking segment.** Small, about 4.7% of bookings per Week 5, but extreme at 73% risk, and now confirmed by the model as a real interaction rather than a coincidence of two weak effects.
3. **Prior no-show history (KPI 1).** Solid for brackets 0-2. Treat the "3+" bracket's exact number as directional only.
4. **Cancellation-to-no-show ratio shifting with lead time (KPI 5).** Validated, useful as an early-warning signal before attendance itself improves.
5. **Distance (KPI 3).** Downgraded to a lower-confidence, still-plausible lever. Worth a small pilot, not a full rollout.
6. **Reminders (KPI 4).** Confirmed weak. Keep running, don't lead with it.

## 6. Translating findings into HealthConnect actions

1. **Lead-time policy.** Require a reconfirmation step for bookings 31+ days out, the point where no-show risk crosses 50%. Confidence: high, validated twice over.
2. **Double-risk flag at booking.** Combine a prior no-show count of 2 or more with a lead time of 46+ days into one flag that triggers a confirmation call, not just an automated reminder. Confidence: high.
3. **Prior no-show risk banding.** Use brackets 0/1/2 with confidence. Treat "3+" as high risk without over-specifying the exact rate. Confidence: high for the banding, low for the precise "3+" number.
4. **Distance-based support.** Pilot transport assistance or a telehealth option for the 15km+ segment on a small scale, with its own measurement plan, rather than a clinic-wide rollout. Confidence: downgraded this week, treat as a hypothesis rather than a certainty.
5. **Keep the reminder programme running** as a supporting measure, not the primary fix. Confidence: unchanged, low but real.

## 7. Evidence behind these recommendations

Week 5's point estimates and confounding check established the three core variables are independent of each other. This week's bootstrap validation of all five KPIs (`KPI_Validation.pdf`) added confidence intervals to each one. The Data Science track's cross-validated model comparison and permutation importance analysis (`cross_track_evidence/Model_Improvement_Validation.pdf`) tested the same questions from a different angle. The strongest piece of evidence in this report is that two of those methods, bootstrap confidence intervals and permutation importance, were designed and run completely separately and still converged on the same downgrade for distance.

## 8. Data limitations

The dataset is fictional and synthetic, so these patterns describe this simulated population, not real clinical behaviour. The "3+" prior no-show bracket is small, 93 rows, 1.9% of the dataset, so its point estimate should be read as directional rather than precise. Distance has about 1.8% missingness, excluded rather than imputed per the Week 5 decision; that's not what's driving the distance finding's weakness (only 90 rows are affected), but it does widen that KPI's confidence intervals somewhat.

None of these are causal claims. Long lead time predicting a no-show doesn't mean shortening the lead time would cause better attendance. It means the two are correlated in a way that supports a targeting decision (who to prioritise for reconfirmation), not a proven lever.

And one thing is still genuinely unresolved: neither this report nor the Data Science track has worked out *why* distance underperforms its own point-estimate gradient once it's inside a model. That's flagged for Week 7 below rather than explained away.

## 9. Cross-track contribution

Full documentation in `CrossTrack_Collaboration_Week6.pdf`. In short: this track's Week 4/5 findings defined the entire feature set for the Data Science model, and that model's cross-validated results and permutation importance fed back into this track's own KPI confidence levels. It went both ways, not just one.

## 10. Week 7 analytical testing requirements

1. Investigate why distance underperforms in the model relative to its raw point-estimate gradient. Check for redundancy with lead time or prior no-shows. Do patients who live further away also tend to book further ahead, in a way that lets lead time absorb distance's signal?
2. Re-run the KPI 1 bootstrap with a larger "3+ prior no-shows" sample if more data becomes available, to settle whether it's truly distinguishable from bracket "2".
3. If a real Data Science or Machine Learning Engineering teammate becomes available, have them independently review the feature-selection table and the robustness-check methodology rather than accepting this track's own self-check as final.
4. Track KPI 2 and the double-risk flag's real-world performance if the recommended booking-policy changes get piloted, to confirm the association holds under an actual intervention, not just in retrospective data.
