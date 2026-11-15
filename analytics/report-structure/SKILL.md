---
name: report-structure
description: Standard structures for analytics reports - one-off analysis write-ups, recurring
  (weekly/monthly) business reviews, and short answers to ad-hoc questions - with
  executive-summary-first layout, chart and table conventions, and a checklist before sharing.
  Use when writing up any analysis, building a recurring report, or asked "can you write
  this up".
---

# Report structure

Goal: a reader who stops after the first few lines still gets the answer; a reader who keeps
going finds the evidence; a reader months later can reproduce it.

## Pick the format first

| Format | When | Length |
|---|---|---|
| **Quick answer** | An ad-hoc question in chat or email | 3-6 lines |
| **Analysis report** | A one-off deep dive that informs a decision | 1-3 pages + appendix |
| **Recurring report** | Weekly or monthly business review of the same metrics | 1 page, same layout every time |

Templates: [templates/analysis-report.md](templates/analysis-report.md),
[templates/recurring-report.md](templates/recurring-report.md).

## Core principles

1. **Answer first.** Open with the conclusion and the recommendation, not the method.
2. **One message per section and per chart.** Title each with its takeaway ("Mobile drove 80%
   of the drop"), not its contents ("Orders by platform").
3. **Every number has a comparison.** vs last period, vs last year, vs target, or vs control.
4. **Pyramid order.** Summary → key findings → supporting detail → method → appendix.
5. **Separate facts from interpretation.** "Conversion fell 5%" is a finding; "probably due
   to the new checkout" is an interpretation; say which is which.
6. **Write for the reader's decision.** Executives need the "so what" and the ask; analysts
   need the method and caveats. Put each where that reader will look.
7. **Reproducible.** Always include the data source (query, sheet, raw data), metric
   definitions and the date range, so anyone can rerun or extend it.

## Quick answer format

```
<Direct answer with the key number and comparison.>
<One line of context or the main driver.>
<Caveat, if it changes how the number should be used.>
Source: <query / dashboard link>, <date range>.
```

## Analysis report sections

1. **TL;DR** (3-5 bullets): the answer, the size of the effect, confidence, the recommendation.
2. **Context**: the question, who asked, and the decision it informs.
3. **Key findings**: 3-5 findings, each with a takeaway title, one chart or table, and two or
   three sentences of explanation.
4. **Recommendation and next steps**: concrete actions with owners, plus open questions.
5. **Caveats and confidence**: what could make this wrong; how sure you are.
6. **Method**: data sources, metric definitions, filters, date range, techniques.
7. **Appendix**: extra cuts, sanity checks, query links.

## Recurring report rules

- Same layout, same metric order, same definitions every period, so readers learn where to look.
- Top: a short summary of what changed and why, written fresh each period.
- Metrics table: current value, change vs last period, change vs last year, vs target, and a
  status (on track / at risk / off track).
- Only explain metrics that moved outside their normal range; don't narrate every line.
- Annotate known events (launches, holidays, outages) next to the numbers they affect.
- Track open items from previous reports until they are closed.
- Note any definition change in the report where it happens, and restate history if needed.

## Charts and tables

- Choose by question: line for trends over time, bar for comparing categories, stacked bar or
  waterfall for contributions to a change, scatter for relationships, table for exact values.
- Title states the takeaway; axis labels include units; note the date range and source.
- Start bar-chart axes at zero. Avoid dual axes, 3D, and pie charts with more than a few slices.
- Highlight what matters (one accent colour) and grey out the rest.
- Format numbers for reading: 1.2M not 1,234,567; consistent decimals; % changes with a sign.
- Use the same colours for the same segments across every chart in the report.

## Before sharing: checklist

- [ ] The TL;DR answers the original question in the first lines.
- [ ] Every number has a comparison and a unit.
- [ ] Numbers match across the summary, charts and tables.
- [ ] Key totals reconciled against a trusted source.
- [ ] Incomplete periods excluded or marked.
- [ ] Caveats stated once, clearly, without burying the conclusion.
- [ ] Recommendations have owners and dates.
- [ ] Data source, definitions and date range included.
- [ ] No confidential data where the report is going.

## Changelog

- 2026-11-14: First version.
