---
name: metric-investigation
description: Step-by-step structure for diagnosing why a metric moved unexpectedly (a drop,
  spike or anomaly) - confirming the change is real, ruling out data issues, locating it by
  segment and time, finding the cause, and reporting it with confidence levels. Use when asked
  "why did X drop/spike", "what happened to X last week", or when an alert fires on a metric.
---

# Metric investigation ("why did X move?")

Goal: find the cause of an unexpected change quickly, and say how sure you are. Work from the
cheapest checks to the most expensive ones, and write down what you ruled out.

## 1. Pin down the change

Answer these in the first few minutes:

- **What exactly moved?** Metric name and definition, the value before and after, and the
  size of the change in absolute and relative terms.
- **Since when?** The first day or hour the change appears. Sudden steps and gradual drifts
  have different causes.
- **Compared with what?** Previous period, same period last year, forecast or normal range.
- **Is it outside normal variation?** Compare with the historical range (e.g. ± 2 standard
  deviations of the same weekday over recent weeks). If it is inside, say so and stop early.

## 2. Rule out data problems first

Most "sudden" changes are data issues. Check in this order:

- [ ] **Freshness**: is the latest data complete, or still arriving?
- [ ] **Pipeline**: failed or delayed jobs, partial loads, backfills, duplicates.
- [ ] **Tracking**: app or web release that changed logging, event renames, broken tags,
      consent or ad-blocker changes.
- [ ] **Definitions**: changed filters, new exclusions, time zone or currency changes.
- [ ] **Dashboard logic**: someone edited the query or the dashboard.
- [ ] **Cross-check**: does an independent source (finance, a second event, server logs)
      show the same change? If not, suspect the data.

## 3. Locate the change

Slice until the change concentrates in one place:

- **Segments**: platform, app version, region, channel, device, browser, customer type,
  new vs returning, product category. Rank segments by their contribution to the total change.
- **Funnel**: which step moved? (visits → product views → add to cart → checkout → purchase)
- **Metric tree**: split the metric into drivers (e.g. revenue = traffic × conversion × AOV)
  and find the driver that moved.
- **Time**: hour-of-day or day-by-day around the start point to find the exact moment.
- **Rate vs mix**: check whether segments changed or the mix between them changed.

A change that is spread evenly across every segment usually points to something global
(a data issue, seasonality, an external event). A change concentrated in one segment points
to something specific to that segment (a release, a campaign, a partner).

## 4. Find the cause

Build a short list of hypotheses and test each against what you found in steps 1-3:

| Category | Examples |
|---|---|
| Internal changes | Product releases, experiments, pricing, promotions ending, marketing spend, ranking or algorithm changes, operational issues (stock-outs, outages) |
| External factors | Seasonality, holidays, weather, competitor actions, news, platform changes (app store, search engine, ad platform) |
| Data issues | Anything that survived step 2 under closer inspection |

For each hypothesis, record: the evidence for, the evidence against, and a verdict
(confirmed / likely / ruled out). Ask the owners of releases and campaigns; check the release
calendar and experiment list rather than guessing.

A cause is **confirmed** when the timing matches, the affected segment matches, the size is
large enough to explain the change, and ideally a reversal or control group shows the effect
going away.

## 5. Size it

- How much of the change does each cause explain? Make the parts add up to the total, and
  show the unexplained remainder honestly.
- What is the impact in business terms (orders, revenue, users) per day or week?
- Is it ongoing, recovering, or a one-off?

## 6. Report

Send a short update early (even "it's not a data issue, still investigating") and a final
summary when done:

```
**What happened**: <metric> fell 8% WoW (from A to B) starting <date>.
**Cause**: <cause> (confidence: high/medium/low). <one-line evidence>.
**Where**: concentrated in <segment>, which accounts for ~N% of the drop.
**Ruled out**: data pipeline, tracking change, seasonality (<one line each>).
**Impact**: ~<N> orders / <amount> per week while it lasts.
**Next steps**: <owner> to <action> by <date>; I'll re-check on <date>.
**Data source**: <query / notebook / dashboard link>.
```

## Common mistakes

- Starting with complex analysis before checking whether the data is broken.
- Stopping at the first plausible story without testing it against the segment breakdown.
- Treating normal variation as an incident.
- Reporting the cause without its size, or without saying what share it explains.
- Not recording what was ruled out, so the next person repeats the work.

## Changelog

- 2026-11-14: First version.
