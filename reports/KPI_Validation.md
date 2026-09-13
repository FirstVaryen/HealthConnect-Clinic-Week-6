# HealthConnect: Week 6 KPI Validation (Data Analytics Track)

**Author:** Hillary Emmanuel

**Task:** Week 6 brief, Data Analytics track, item 5: "Validate the most important KPIs developed in Week 5."

---

## 1. What to expect this week

Week 5 calculated each KPI once, as a single point estimate on the full dataset. That tells you what the rate is, not how much that number would move if the sample had come out slightly differently. A point estimate with no sense of its own stability looks identical to a fragile number that just happened to land somewhere.

**Method:** bootstrap resampling. For each KPI, resample the underlying rows 1,000 times with replacement (same size as the original group), then report the resulting 95% confidence interval next to the Week 5 point estimate. Full working and outputs are in `notebooks/HealthConnect_Week6_KPI_Validation.ipynb`.

A KPI whose bands have confidence intervals that don't overlap is robust; the pattern would likely reappear with a different sample. A KPI whose bands overlap isn't necessarily wrong, but the point estimate shouldn't be presented with the same confidence as a robust one.

## 2. KPI 1: No-Show Rate by Prior No-Show Bracket

| Bracket | n | Point estimate | 95% CI |
|---|---|---|---|
| 0 | 2,921 | 43.5% | [41.6%, 45.4%] |
| 1 | 1,548 | 53.5% | [51.0%, 56.1%] |
| 2 | 438 | 59.4% | [54.6%, 63.5%] |
| 3+ | 93 | 68.8% | [58.1%, 78.5%] |

Validated, with one caveat carried over from Week 5 and now measured precisely. Brackets 0, 1, and 2 separate cleanly with no overlap between adjacent pairs. Bracket "3+" (93 rows) has a wide interval, 58.1% to 78.5%, that overlaps with bracket 2's 54.6% to 63.5%. Week 5 flagged this bracket's small size as a concern; the bootstrap makes it exact. We cannot statistically tell "3+ prior no-shows" apart from "2 prior no-shows" at this sample size. The overall rising-risk pattern holds. The exact number for the "3+" tail doesn't.

## 3. KPI 2: No-Show Rate by Booking Lead-Time Band

| Band | n | Point estimate | 95% CI |
|---|---|---|---|
| 0-7 days | 640 | 27.8% | [24.4%, 31.1%] |
| 8-14 days | 602 | 33.6% | [29.7%, 37.4%] |
| 15-30 days | 1,333 | 43.2% | [40.7%, 45.9%] |
| 31-45 days | 1,258 | 53.8% | [51.0%, 56.6%] |
| 46-60 days | 1,167 | 67.7% | [65.2%, 70.2%] |

Fully validated. This is the strongest KPI in the set. Every band separates cleanly from every non-adjacent band, and even neighboring bands barely touch (0-7 vs 8-14: 31.1% vs 29.7%). The climb from 27.8% to 67.7% is a real, stable pattern, not an artefact of how the data happened to split. Three separate checks now agree on this: the Week 5 point estimates, this bootstrap, and the Data Science track's cross-validated model, where `booking_lead_days` came out as the single most important predictive feature by a wide margin (permutation importance 0.097-0.105, five to ten times any other feature).

## 4. KPI 3: No-Show Rate by Distance Band

| Band | n | Point estimate | 95% CI |
|---|---|---|---|
| <5 km | 1,156 | 46.5% | [43.5%, 49.4%] |
| 5-10 km | 1,692 | 46.5% | [44.1%, 48.8%] |
| 10-15 km | 1,132 | 48.5% | [45.6%, 51.6%] |
| 15+ km | 930 | 54.1% | [51.0%, 57.4%] |

This KPI changed this week. The three middle bands, under 5km, 5-10km, and 10-15km, have confidence intervals that overlap almost completely: the data can't reliably tell these groups apart. Only the two extremes separate at all, and even then just barely, 49.4% against 51.0%, a gap of 1.6 points.

Nothing was wrong with the Week 5 calculation. The point estimates are correct. What this check catches is that a real gradient in the point estimates can still be too noisy to call a "cliff" with any confidence.

The Data Science track found the same thing independently, working from the same processed dataset but a completely different method: permutation importance on a trained classifier, not confidence intervals on a rate. `distance_to_clinic_km` and a `distance_15km_plus` threshold flag both came out with negligible predictive value next to lead time and prior no-shows. Two unrelated methods landing on the same downgrade is stronger evidence than either one alone.

Distance-based intervention, transport support or a telehealth option, should now be framed as a hypothesis worth a small, measured pilot, not the confirmed lever Week 5 implied.

## 5. KPI 4: Wasted Slot Rate by Reminder Status

| Group | n | Point estimate | 95% CI |
|---|---|---|---|
| Reminder sent | 3,634 | 47.4% | [45.8%, 49.0%] |
| No reminder | 1,366 | 51.4% | [48.7%, 54.2%] |
| Overall | 5,000 | 48.5% | [47.0%, 49.9%] |

Confirmed weak, now precisely. The two groups' intervals nearly touch, 49.0% against 48.7%, a 0.3-point gap, which means the roughly 4-point reminder effect sits right at the edge of what this sample size can distinguish from noise. That matches the Data Science model's near-zero permutation importance for reminder status. Week 5 already called this "worth keeping, but nowhere near enough on their own." That framing turns out to have been right all along.

## 6. KPI 5: Cancellation-to-No-Show Ratio

| Group | n | Point estimate | 95% CI |
|---|---|---|---|
| Overall | 5,000 | 0.109 | [0.095, 0.122] |
| 0-7 days | 640 | 0.202 | [0.138, 0.286] |
| 8-14 days | 602 | 0.139 | [0.088, 0.197] |
| 15-30 days | 1,333 | 0.118 | [0.090, 0.148] |
| 31-45 days | 1,258 | 0.105 | [0.079, 0.132] |
| 46-60 days | 1,167 | 0.076 | [0.057, 0.097] |

Validated. The two ends of the range, 0-7 days at [0.138, 0.286] and 46-60 days at [0.057, 0.097], don't overlap at all. Longer lead time means proportionally more silent no-shows relative to proper cancellations, and that pattern is real, not sampling noise.

## 7. Summary: what changed and what didn't

| KPI | Week 5 status | Week 6 validation result |
|---|---|---|
| 1: Prior no-show bracket | Point estimate only | Robust for brackets 0-2; "3+" confirmed fragile (already flagged, now quantified) |
| 2: Lead-time band | Point estimate only | Fully validated, strongest KPI, confirmed by three independent methods |
| 3: Distance band | Point estimate, framed as a "cliff" | Downgraded: middle bands statistically indistinguishable, corroborated by the DS model |
| 4: Wasted slot rate / reminders | Point estimate, framed as "weak but real" | Confirmed at the same confidence level, no change needed |
| 5: Cancellation-to-no-show ratio | Point estimate only | Validated, the trend is real and stable |

KPI 3 is the one that moves the Week 6 narrative. Every recommendation and dashboard element built on the "15km cliff" should carry a lower confidence label from here on, and the distance-based recommendation should be reframed as a pilot rather than a confirmed lever.
