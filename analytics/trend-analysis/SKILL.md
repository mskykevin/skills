---
name: trend-analysis
description: Structure for analysing how a metric changes over time - describing the trend,
  separating real change from seasonality, noise and data artifacts, breaking it down by
  segment, and making a simple forecast. Use when asked "how is X trending", "is this growth
  real", "what's the outlook for X", or when preparing the time-series part of any report.
---

# Trend analysis

Goal: say what the trend is, whether it is real, what is driving it, and what to expect next,
with the evidence for each.

## 1. Frame the question

Write these down before querying anything:

- **Metric and definition**: numerator, denominator, grain, filters, time zone.
- **Time window**: long enough to see at least two full seasonal cycles where possible
  (e.g. 2+ years for yearly seasonality, 8+ weeks for weekly patterns).
- **Granularity**: daily for operational questions, weekly for most product questions,
  monthly for business reviews. Weekly smooths day-of-week noise; say which you chose and why.
- **The decision**: what someone will do differently depending on the answer.

## 2. Check the series before reading it

Look for artifacts that fake a trend:

- [ ] Gaps or zero days (pipeline outages, late-arriving data). Exclude incomplete recent
      periods; mark the last complete period explicitly.
- [ ] Definition or logging changes (new event version, changed filter, backfills).
- [ ] Step changes on a single day: almost always a data or product change, not organic.
- [ ] Mix changes: a new market, platform or customer segment entering the data.
- [ ] Ratio metrics: check numerator and denominator separately. A rising rate can be a
      shrinking denominator.

If you find an artifact, fix it or annotate it on the chart. Never present an uncorrected
artifact as a trend.

## 3. Describe the trend

Use one standard set of views:

| View | Shows |
|---|---|
| Raw series with a rolling average (7-day for daily data, 4-week for weekly) | The overall shape |
| Period-over-period change (WoW, MoM) | Short-term momentum (noisy, use with care) |
| Year-over-year change (same period last year, aligned by weekday) | Change with seasonality removed |
| Indexed to 100 at a start date | Compare segments or metrics of different sizes |
| Cumulative / year-to-date vs target | Progress toward a goal |

State the trend in one sentence with numbers: direction, size, and period. For example:
"Weekly orders grew 12% YoY in Q3, up from 8% in Q2; growth is accelerating."

## 4. Separate signal from noise and seasonality

- **Seasonality**: compare YoY, or decompose the series (e.g. STL) into trend, seasonal and
  residual parts. Account for holidays and calendar shifts (Easter, Lunar New Year, a 53rd week).
- **Noise**: estimate the normal range from history (e.g. mean ± 2 standard deviations of
  weekly changes over the last 6-12 months). A change inside that range is not a story yet.
- **Real change**: sustained for several periods, larger than normal variation, and visible
  in more than one related metric (e.g. both sessions and orders).
- **Known events**: annotate launches, campaigns, pricing changes, outages and external events
  on the chart. Ask the owners of those events; don't guess.

## 5. Decompose: what is driving it

Break the change into parts so it can be acted on:

- **By segment**: region, platform, channel, customer type, new vs returning. Find which
  segments account for most of the change (contribution = segment change ÷ total change).
- **Rate vs mix**: did segments change, or did the mix shift toward segments with different
  levels? (Simpson's paradox lives here.) Report both effects when the mix moved.
- **Metric tree**: split the metric into its drivers, e.g. revenue = users × conversion ×
  orders per buyer × average order value, and show which driver moved.
- **Cohorts**: for user metrics, compare cohorts by start month to see if newer cohorts behave
  differently from older ones.

## 6. Look ahead (only if asked or useful)

- Start simple: last year's value × current YoY growth, or a seasonal naive forecast.
  Use a proper model (e.g. Prophet, ETS, ARIMA) only when the simple method is clearly not enough.
- Always give a range, not a point: e.g. low / base / high, with the assumptions for each.
- Backtest: check how the method would have done on the last few periods.
- Say what would make the forecast wrong (a planned launch, a pricing change, macro events).

## 7. Output

Lead with the answer, then the evidence:

1. **Headline**: the trend in one sentence with numbers and the comparison.
2. **Is it real**: artifacts checked, seasonality handled, size vs normal variation.
3. **Drivers**: the segments or metric-tree components that explain most of the change.
4. **Outlook** (optional): range plus assumptions.
5. **So what**: implication for the decision, and recommended next step.
6. **Data source**: query or notebook link, metric definition, last complete period.

Chart conventions: time on the x-axis, annotate events directly on the chart, start the
y-axis at zero for counts (not necessarily for rates), and mark incomplete periods.

## Common mistakes

- Reading a trend from one or two data points, or from an incomplete current period.
- Comparing a holiday week to a normal week.
- Reporting WoW changes for a metric with strong weekly or monthly cycles.
- Missing a mix shift, so every segment looks flat while the total moves.
- Calling a definition change "growth".

## Changelog

- 2026-11-14: First version.
