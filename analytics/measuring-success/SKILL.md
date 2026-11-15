---
name: measuring-success
description: Framework for defining how success is measured for a product, feature, launch,
  campaign or team - from goal to metric tree, choosing primary, secondary and guardrail
  metrics, setting baselines and targets, and evaluating impact with or without an A/B test.
  Use when asked "how do we know this is working", "what should our success metrics be",
  "did the launch work", or when writing the measurement plan for a project.
---

# Measuring success

Goal: agree *before* launch what success looks like and how it will be measured, then judge
the result against that plan, not against whatever number looks best afterwards.

## 1. Start from the goal

Write one sentence for each:

- **Objective**: what we want to change, for whom. ("New customers find a product they want
  faster.")
- **Why it matters**: the business outcome it serves (retention, revenue, cost, satisfaction).
- **Decision**: what we will do depending on the result (ship, iterate, roll back, invest more).

## 2. Build the metric tree

Connect the work to the business outcome through a chain of metrics:

```
Business outcome (lagging, slow):      revenue, retention, profit
        ↑
Product / team metric (the target):    e.g. weekly purchasing customers
        ↑
Driver metrics (leading, fast):        e.g. search success rate, add-to-cart rate
        ↑
The thing we change:                   feature, campaign, process
```

Leading metrics move quickly and can be influenced directly; lagging metrics are what the
business ultimately cares about. Measure both: leading to learn fast, lagging to confirm value.

## 3. Choose the metrics

| Role | How many | Purpose | Example |
|---|---|---|---|
| **Primary (success) metric** | One | The single number the decision rests on | Conversion rate of new visitors |
| **Secondary metrics** | 2-4 | Explain *why* the primary moved; show the mechanism | Search usage, add-to-cart rate |
| **Guardrail metrics** | 2-4 | Must not get worse; protect the business and users | Refund rate, page load time, unsubscribe rate |
| **Health / counter metrics** | As needed | Catch gaming or side effects | Revenue per order (if conversion is the primary) |

Test every metric against these questions:

- [ ] **Sensitive**: can the change realistically move it within the measurement window?
- [ ] **Attributable**: can we tell our change apart from everything else moving it?
- [ ] **Hard to game**: would hitting the number without real improvement be obvious?
- [ ] **Defined precisely**: numerator, denominator, grain, filters, window (e.g. "within 7
      days of first visit").
- [ ] **Measurable today**: the tracking exists, or there is a plan to add it before launch.

Prefer rates and per-user metrics over raw totals, which grow with traffic regardless of quality.

## 4. Baseline and target

- **Baseline**: the current value and its normal variation over a comparable period
  (including seasonality).
- **Minimum meaningful change**: the smallest change worth acting on, in business terms.
- **Target**: grounded in history, benchmarks, or the size of the opportunity; not a round
  number picked for a slide. Say whether it is a commitment or a stretch goal.
- **Time frame**: when we will read the result, and how long effects need to show up
  (novelty effects fade; retention takes weeks).

## 5. Evaluate impact

Pick the strongest method the situation allows, and state which one you used:

| Situation | Method | Strength |
|---|---|---|
| Can randomise users | A/B test with a pre-registered primary metric and sample size (see `data-science/experiment-design`) | Strongest |
| Rollout by region, store or time | Difference-in-differences against regions or groups not yet launched | Good, if pre-trends are parallel |
| Launch to everyone at once | Interrupted time series / before-after against a forecast of what would have happened | Medium |
| Compare users who used the feature vs not | Matching or regression on pre-launch behaviour | Weak (self-selection); say so |
| Only a before/after average | Report it as descriptive only | Weakest; don't claim causation |

For any method:

- Compare with the baseline *and* with what would have happened anyway (the counterfactual).
- Check guardrails as carefully as the primary metric.
- Look at key segments, but treat segment wins found after the fact as hypotheses, not results.
- Report the effect with a confidence interval or a plausible range, not just a point estimate.

## 6. Write the measurement plan (before launch)

```
Objective:
Decision it informs:
Primary metric (definition, baseline, minimum meaningful change, target):
Secondary metrics:
Guardrail metrics (and the threshold that would stop the launch):
Method (A/B / diff-in-diff / time series / descriptive) and why:
Measurement window and read-out date:
Tracking needed and owner:
Data source:
```

Get the plan agreed with the stakeholders before launch. Changing the primary metric after
seeing results is the most common way success reports go wrong.

## 7. Report the result

1. **Verdict**: success / no effect / negative, against the pre-agreed target.
2. **Primary metric**: effect size, range, and method.
3. **Mechanism**: what the secondary metrics show.
4. **Guardrails**: all clear, or which one moved and by how much.
5. **Recommendation**: ship, iterate, roll back, or measure longer.
6. **Data source**: queries, dashboards, and the measurement plan it was judged against.

## Common mistakes

- Choosing metrics after the launch, or switching to the metric that happened to go up.
- Vanity metrics (page views, sign-ups) with no link to the business outcome.
- Too many "primary" metrics, so something always looks like a win.
- No guardrails, so a win on one number hides a loss on another.
- Claiming causation from a before/after comparison during a seasonal period.
- Reading results before the planned window ends and stopping when they look good.

## Changelog

- 2026-11-14: First version.
