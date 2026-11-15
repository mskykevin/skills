# Analytics

Playbooks for the analytics questions that come up again and again, whatever the company or
product. Each skill gives a fixed structure for one *type* of question, so an agent (or I) can
answer it the same careful way every time.

| Question you're asked | Skill |
|---|---|
| "How is X trending?" / "Is this growth real?" / "What will X be next quarter?" | [trend-analysis](trend-analysis/SKILL.md) |
| "Why did X drop (or spike) last week?" | [metric-investigation](metric-investigation/SKILL.md) |
| "How do we know this is working?" / "What should our success metrics be?" / "Did the launch work?" | [measuring-success](measuring-success/SKILL.md) |
| "Can you write this up?" / "Build me a weekly report" | [report-structure](report-structure/SKILL.md) |

## Rules that apply to every analytics skill

1. **Start from the decision.** Before touching data, write down who is asking, what decision
   the answer informs, and what would change that decision. If nobody can name a decision, say
   so and keep the work small.
2. **Define every metric before you compute it.** Numerator, denominator, grain, filters, time
   zone and date range. Write the definition next to the number.
3. **Sanity-check the data first.** Row counts, duplicates on join keys, null rates, date
   coverage, and one number reconciled against a trusted source (a dashboard, finance, a
   previous report). A result built on an unchecked join is not a result.
4. **Compare against something.** A number alone means nothing. Always show it against a
   baseline: previous period, same period last year, target, or a control group.
5. **Separate what the data shows from what you think it means.** Label observations,
   interpretations and recommendations as such.
6. **State confidence and caveats plainly.** Say how sure you are and what could make the
   conclusion wrong, in one or two sentences, not a wall of disclaimers.
7. **Always include the data source** (query, sheet, raw data) for future reference, so anyone
   can reproduce or extend the result.
8. **No confidential specifics in these skills.** They describe patterns. Company metric
   definitions, table names and numbers stay in the company's own systems.

## How these relate to other folders

- `data-science/analysis-workflow` (planned) is the general question → answer process; these
  skills are the playbooks for specific question types inside it.
- `data-science/experiment-design` (planned) covers A/B tests in depth. `measuring-success`
  covers choosing the metrics and what to do when there is no experiment.
- `communication/analysis-writeup` (planned) is about prose style; `report-structure` is about
  the shape of analytics reports and recurring dashboards-in-documents.
- [work-style/analysis.md](../work-style/analysis.md) holds my personal notes; settled rules
  from there graduate into these skills.
