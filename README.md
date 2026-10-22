# Skills

A personal, portable library of **agent skills**: my work style, written down so an AI agent
(Claude Code or similar) can follow it from day one of anything new.

## Why this exists

I work across very different contexts: data science at Uber, my e-commerce project, and
whatever comes next (a new company, a new side project). Each time I start fresh I rebuild the
same habits by hand: how I lay out agent folders, how I track progress, how I write up an
analysis, how I hand work off between sessions.

This folder is the **jump start**. Starting something new should take minutes:

1. Point the new project at these skills (see [Using these skills](#using-these-skills)).
2. Run the bootstrap skill to scaffold the project the way I like it.
3. Add only the project-specific context on top.

### Design principles

- **General, not specific.** A skill here should be useful in at least two of my contexts. If
  it only makes sense at one company or in one repo, it belongs in that repo's own
  `.claude/skills/` directory, not here.
- **No confidential data.** Never put employer code, table names, metric definitions, internal
  URLs, or data in this repo. Write the *pattern* ("how I define and validate a metric") and keep
  the *instance* ("Uber's trip-completion metric") inside the company's systems.
- **Opinionated and short.** A skill is a set of instructions, not an essay. If a skill is
  longer than about 300 lines, split it or move the details into `references/`.
- **Living documents.** When I catch myself correcting an agent the same way twice, that
  correction goes into a skill.

---

## Proposed structure

This repository *is* the skills folder; the tree below is its root.

```
skills/
├── README.md                    # this file
├── _template/                   # copy this to start a new skill
│   └── SKILL.md
├── resources/                  # reading lists that inform skills (not skills)
│   ├── git-and-agent-best-practices.md
│   └── orchestrator-subagent-best-practices.md
├── work-style/                  # personal notes on how I work (not skills)
│   └── analysis.md
│
├── workflow/                    # how I work, whatever the domain
│   ├── project-bootstrap/       # scaffold a new project/repo in my standard layout
│   ├── progress-tracking/       # keep PROGRESS.md / worklog up to date
│   ├── session-handoff/         # end-of-session summary so the next session can resume
│   ├── task-planning/           # break a goal into a plan before executing
│   └── weekly-review/           # roll up the week: done, blocked, next
│
├── engineering/                 # general software habits
│   ├── git-conventions/         # branch names, commit messages, PR descriptions
│   ├── code-review/             # my review checklist
│   └── python-project-setup/    # uv/poetry, ruff, pytest, pre-commit defaults
│
├── data-science/                # DS/ML work style (no company specifics)
│   ├── analysis-workflow/       # question -> hypothesis -> data -> result -> writeup
│   ├── sql-style/               # formatting, CTE conventions, validation queries
│   ├── experiment-design/       # A/B test design, power, guardrails, readout template
│   ├── metric-definition/       # how to define, document and sanity-check a metric
│   ├── notebook-hygiene/        # notebook layout, when to promote code to modules
│   └── ml-model-lifecycle/      # baseline first, eval plan, model card, monitoring
│
├── communication/               # writing for humans
│   ├── analysis-writeup/        # executive-summary-first doc template
│   ├── stakeholder-update/      # short status update format
│   └── decision-doc/            # options, trade-offs, recommendation
│
└── onboarding/                  # ramping up somewhere new
    ├── new-codebase-ramp-up/    # how to explore and map an unfamiliar repo
    └── new-role-30-60-90/       # questions to ask, people to meet, notes to keep
```

### Anatomy of a single skill

Each skill is a folder with a `SKILL.md` file plus optional supporting files:

```
progress-tracking/
├── SKILL.md            # required: frontmatter + instructions
├── templates/          # files the skill copies into a project (e.g. PROGRESS.md)
├── references/         # longer docs the agent reads only when needed
└── scripts/            # optional helper scripts
```

`SKILL.md` starts with YAML frontmatter. The `description` is what the agent uses to decide
*when* to load the skill, so it should say both what the skill does and when to use it:

```markdown
---
name: progress-tracking
description: Maintain PROGRESS.md and a dated worklog for the current project. Use at the
  start of a session to load context, after finishing a task, and before ending a session.
---

# Progress tracking

## Files
- `PROGRESS.md` at the repo root: current goal, active tasks, blockers, next steps.
- `worklog/YYYY-MM-DD.md`: what was done that day, decisions made, open questions.

## Rules
1. At session start, read PROGRESS.md and the latest worklog entry before doing anything.
2. After each meaningful task, update the task status in PROGRESS.md.
3. Record *decisions and why*, not just actions.
...
```

---

## Using these skills

Claude Code looks for skills in two places:

| Scope | Location | Use for |
|-------|----------|---------|
| Personal (every project on this machine) | `~/.claude/skills/<skill-name>/` | Everything in this folder |
| Project (shared with that repo's team) | `<repo>/.claude/skills/<skill-name>/` | Project-specific skills, or a copy of a general one |

Skills are found one level deep, so the category folders above (`workflow/`, `data-science/`,
...) have to be flattened when installing. A small script does that. It is the next thing to
add here:

```bash
# install.sh at the repo root (planned): symlink every skill into ~/.claude/skills
for dir in "$(dirname "$0")"/*/*/; do
  [ -f "$dir/SKILL.md" ] && ln -sfn "$(realpath "$dir")" ~/.claude/skills/"$(basename "$dir")"
done
```

With symlinks, a `git pull` of this repo updates the skills everywhere at once.

On a work laptop where personal repos can't be cloned, copy the relevant skills into the
project's `.claude/skills/` instead, and check that this follows the company's policy.

---

## Where to start

Don't build all of the above at once. Start with the skills that pay off on day one of any new
project, then add more as I notice repeated corrections.

### Phase 1: the core loop (build first)

| Skill | Why it's high value |
|-------|---------------------|
| **project-bootstrap** | One command to create my standard layout: `CLAUDE.md`, `PROGRESS.md`, `worklog/`, `docs/decisions/`, `.claude/` folder, `.gitignore`. This is the "how I organize my agent folders" skill and it turns the rest into habits. |
| **progress-tracking** | Defines the `PROGRESS.md` + dated worklog format and when the agent must read or update it. Long-running work stays resumable across sessions and context resets. |
| **session-handoff** | End-of-session ritual: what changed, what's half-done, what's next, the exact command to resume. Pairs with progress-tracking. |
| **claude-md-authoring** | How I write a project's `CLAUDE.md`: purpose, layout, how to run and test, conventions, gotchas. Every new project needs one. |

### Phase 2: daily craft (build in the first few weeks)

| Skill | Why it's high value |
|-------|---------------------|
| **analysis-workflow** | My default path from a vague question to a defensible answer: clarify the decision, state hypotheses, check the data, analyse, write up. Applies to Uber and to e-commerce metrics alike. |
| **sql-style** | Formatting and CTE conventions, plus mandatory sanity checks (row counts, duplicates on join keys, null rates, date-range coverage). Catches the most common silent errors. |
| **analysis-writeup** | Executive summary first, then method, results, caveats and next steps. Makes every result shareable. |
| **git-conventions** | Branch naming, commit message format, PR template. Small, but used constantly. See [Git + AI agent best practices](resources/git-and-agent-best-practices.md) for sources to borrow from. |

### Phase 3: depth (add when the need shows up)

- **experiment-design**: hypothesis, primary metric, guardrails, power and sample size,
  readout template.
- **metric-definition**: definition, grain, filters, owner, known pitfalls, validation query.
- **ml-model-lifecycle**: baseline first, offline eval plan, model card, monitoring checklist.
- **new-codebase-ramp-up**: map an unfamiliar repo (entry points, data flow, tests) into a
  `docs/codebase-map.md`.
- **new-role-30-60-90**: onboarding note structure, a questions-to-ask list, a people map.
- **weekly-review** and **stakeholder-update**: turn the worklog into a weekly summary and a
  short status message.

### Suggested standard project layout

This is the layout that `project-bootstrap` would create:

```
<project>/
├── CLAUDE.md               # agent instructions for this project
├── PROGRESS.md             # live status: goal, tasks, blockers, next steps
├── worklog/
│   └── 2026-10-16.md       # one file per working day
├── docs/
│   ├── decisions/          # short ADRs: 0001-use-duckdb.md
│   └── notes/              # research, meeting notes
├── .claude/
│   ├── skills/             # project-specific skills only
│   └── settings.json       # project permissions / hooks
└── src/ | notebooks/ | sql/
```

---

## Resources

Reading lists that inform the skills here (no `SKILL.md`, so they are not installed):

- [Git + AI agent best practices](resources/git-and-agent-best-practices.md): Git courses and
  cheat sheets, commit conventions, official Claude Code best-practice guides, and popular
  community Agent Skills for Git workflows.
- [Orchestrator + subagent best practices](resources/orchestrator-subagent-best-practices.md):
  how to split work between a lead agent and focused subagents (when to fan out, writing
  delegation briefs, designing subagents, subagents vs agent teams vs worktrees), plus official
  guides, the multi-agent debate, and GitHub skills and frameworks to borrow from.

## Work style

Personal notes summarising how I work, kept for future reference (no `SKILL.md`, so they are not
installed). Settled rules from here can graduate into skills:

- [Analysis](work-style/analysis.md): how I frame, run, validate and communicate an analysis.

---

## Conventions for this folder

- One skill per folder; folder name = `name` in the frontmatter, kebab-case.
- Write instructions as direct rules ("Always read PROGRESS.md first"), not background prose.
- Put long examples and templates in subfolders so `SKILL.md` stays short.
- Record notable changes to a skill in a short "Changelog" section at the bottom of its
  `SKILL.md`.
- Every few months, prune skills I no longer use and merge ones that overlap.
