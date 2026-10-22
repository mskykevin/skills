# Orchestrator + subagent best practices

How to use an **orchestrator → subagent hierarchy** to get more done with an agent framework
(Claude Code, the Claude Agent SDK, or similar): one lead agent plans, splits, and checks the
work; focused helper agents do the pieces in their own clean context.

This is a reading list plus my working notes, not a skill: it has no `SKILL.md`, so the install
script skips it. Lessons that I keep re-applying should graduate into a skill (e.g.
`workflow/task-planning` or a future `workflow/delegation`).

Star counts and doc details as of Oct 2026.

---

## 1. 🧭 The mental model

```
                 ┌──────────────────────────────┐
   me  ───────▶  │  Orchestrator (main session)  │  plans · splits · delegates · verifies · merges
                 └──────┬───────────┬───────────┘
          brief ▼       │ brief ▼   │ brief ▼        (each brief is self-contained)
          ┌─────────┐ ┌─────────┐ ┌─────────┐
          │ worker  │ │ worker  │ │ worker  │       own context window, scoped tools,
          │ explore │ │ test    │ │ review  │       cheaper model where it's enough
          └────┬────┘ └────┬────┘ └────┬────┘
               └─ short summary ─┴── back to the orchestrator ──┘
```

Why it pays off:

- **Context stays clean.** Noisy work (searching a big repo, reading logs, running tests) happens
  in a subagent; only the summary comes back. The orchestrator keeps its context for decisions.
- **Parallelism.** Independent questions get answered at the same time instead of one after another.
- **Specialisation.** Each worker gets its own prompt, tool allowlist and model.

What it costs:

- **Tokens.** Anthropic measured agents at about **4×** the tokens of a chat, and multi-agent
  systems at about **15×**. Worth it only when the task is valuable and splits cleanly.
- **Lost context.** A subagent does **not** see my conversation. Anything it needs has to be in
  the brief. Most multi-agent failures are briefing failures.
- **Conflicting work.** Parallel *writers* make conflicting implicit decisions (Cognition's
  "Flappy Bird" example). Parallel *readers* are much safer.

## 2. ✅ When to use it (and when not to)

| Good fit: delegate | Poor fit: stay in one session |
| --- | --- |
| Broad, independent research questions ("how do modules A, B, C handle auth?") | Tightly coupled edits where every step depends on the last |
| High-volume output I only need a summary of (test runs, log triage, repo search) | Quick, targeted edits (a one-file fix) |
| Review from several separate lenses (security, performance, tests) | Lots of back-and-forth with me to clarify intent |
| Debugging with competing hypotheses | Several agents editing the **same files** |
| Repeated chores with a fixed recipe (lint fixes, migrations per module) | Latency-sensitive work: subagents start cold and need time to gather context |

**Rule of thumb:** many agents may *read and think*; keep *writes* single-threaded, or give each
writer its own files or its own git worktree.

## 3. 🛠️ Best practices

### Orchestrator: plan, delegate, verify

1. **Plan first, then split.** Have the orchestrator write the plan (or use plan mode) before any
   worker starts. Split along boundaries that don't share state: modules, files, questions.
2. **Scale effort to the task.** Anthropic's research system uses 1 agent with 3–10 tool calls for
   simple fact-finding, 2–4 subagents for comparisons, and 10+ only for genuinely complex research.
   Early versions wasted effort by spawning 50 subagents for simple queries.
3. **Launch independent workers in parallel** (in one message), and chain dependent ones in sequence
   (e.g. *explore → implement → review*).
4. **Don't redo the workers' job.** Avoid having the orchestrator re-search what a worker already
   found; it defeats the context savings. Equally, tell it to *wait* for workers before acting.
5. **Verify before reporting done.** The orchestrator owns the final answer: run the tests, read
   the diff, spot-check worker claims. A worker's "done" is a claim, not evidence.
6. **Watch the return traffic.** Ten detailed reports can flood the orchestrator's context. Ask
   workers for a short, structured summary, with details written to a file if needed.

### Writing a good delegation brief

Every brief should stand on its own: objective, output format, tools/sources, and clear
boundaries (Anthropic's list). My template:

```text
Goal: <one sentence: what and why>
Context: <facts the worker can't discover cheaply: repo, branch, constraints, what I've ruled out>
Scope: <files/dirs/questions IN scope> · Out of scope: <what NOT to touch>
Tools: <what to use, e.g. read-only search; don't run migrations>
Done when: <concrete finish line>
Return: <format, e.g. "≤10 bullets: finding · file:line · confidence">
```

- Restate rules that matter (from `CLAUDE.md`, conventions, "don't push") in the brief. Some
  built-in agents (e.g. Claude Code's `Explore`, `Plan`) skip `CLAUDE.md` entirely.
- Prefer good heuristics ("start broad, then narrow") over rigid step lists.

### Designing subagents (Claude Code)

Subagents are Markdown files with YAML frontmatter in `.claude/agents/` (project) or
`~/.claude/agents/` (personal). Only `name` and `description` are required.

```markdown
---
name: code-reviewer
description: Reviews a diff for bugs and risky changes. Use proactively after code changes.
tools: Read, Glob, Grep          # read-only: a reviewer shouldn't edit
model: sonnet                    # cheaper model where it's enough
---

You review code. Report findings as: severity · file:line · problem · suggested fix.
Report only real defects; skip style nits unless asked.
```

- **One job per agent**, with a `description` that says *when* to use it ("use proactively
  after…"). The description is how the orchestrator decides to delegate.
- **Least privilege:** use `tools` / `disallowedTools` to grant only what the job needs. Use a
  `PreToolUse` hook for finer rules (e.g. block writes in a read-only DB agent).
- **Right-size the model:** strong model for the orchestrator, cheaper (`haiku` / `sonnet`) for
  search and routine work. `CLAUDE_CODE_SUBAGENT_MODEL` sets a default for all subagents.
- **Isolate writers:** `isolation: worktree` gives a subagent its own temporary git worktree.
- **Keep nesting shallow.** Subagents can spawn subagents (3 levels by default; set
  `CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH=1` to disable). Leave `Agent` out of a worker's
  `tools` so it can't fan out further.
- **Version the good ones** in the repo's `.claude/agents/` so the team (and future me) reuses them.

### Pick the right level of parallelism

| Option | Use when | Notes |
| --- | --- | --- |
| **Single session** | Coupled or small tasks | Cheapest, simplest. The default. |
| **Subagents** | Focused workers where only the result matters | Results return to the caller; lowest multi-agent cost |
| **Agent teams** (experimental) | Workers need to talk to each other, debate, or share a task list | Higher cost; start with 3–5 teammates and 5–6 tasks each; each teammate owns its own files |
| **Parallel sessions + git worktrees** | I want to run and steer several sessions myself | No automatic coordination; I'm the orchestrator |
| **Agent SDK / headless (`claude -p`)** | Scripted pipelines, CI, scheduled jobs | Define agents in code; same orchestrator→worker pattern |

---

## 4. 📚 Official guides (read these first)

| Resource | Why |
| --- | --- |
| [How we built our multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system) | **The** orchestrator–worker case study: delegation briefs, effort scaling, parallel tool calls, token costs, evaluation |
| [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents) | The basic patterns (prompt chaining, routing, parallelization, **orchestrator-workers**, evaluator-optimizer) and when to stay simple |
| [Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) | Why sub-agent architectures help manage context, along with compaction and note-taking |
| [Writing effective tools for agents](https://www.anthropic.com/engineering/writing-tools-for-agents) | Tool design matters even more when many agents share the tools |
| [Building agents with the Claude Agent SDK](https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk) | The agent loop (gather context → act → verify) and where subagents fit |
| [Claude Code docs: subagents](https://code.claude.com/docs/en/sub-agents) | Built-in agents (`Explore`, `Plan`, `general-purpose`), frontmatter fields, tool scoping, nesting, parallel runs |
| [Claude Code docs: agent teams](https://code.claude.com/docs/en/agent-teams) | Lead + teammates with a shared task list and messaging; when teams beat subagents |
| [Claude Code docs: worktrees](https://code.claude.com/docs/en/worktrees) | Run parallel sessions without file conflicts |
| [Claude Code docs: costs](https://code.claude.com/docs/en/costs) | Token cost of subagents and teams, and how to keep it down |
| [Agent SDK: subagents](https://code.claude.com/docs/en/agent-sdk/subagents) | Define and call subagents programmatically (Python / TypeScript) |
| [anthropics/claude-cookbooks: patterns/agents](https://github.com/anthropics/claude-cookbooks/tree/main/patterns/agents) | Runnable notebooks: `orchestrator_workers.ipynb`, `async_multi_agent_orchestration.ipynb`, `basic_workflows.ipynb` |
| [anthropics/claude-agent-sdk-python](https://github.com/anthropics/claude-agent-sdk-python) (⭐ 8k) | SDK with programmatic subagents and session forking |

## 5. ⚖️ The debate: when multi-agent hurts

Read these together: they disagree on the surface but converge on "context is everything".

| Resource | Takeaway |
| --- | --- |
| [Cognition: Don't Build Multi-Agents](https://cognition.com/blog/dont-build-multi-agents) | Share full context; actions carry implicit decisions, so parallel writers conflict |
| [Cognition: Multi-Agents: What's Actually Working](https://cognition.com/blog/multi-agents-working) (Apr 2026) | Follow-up: many agents contributing intelligence works if **writes stay single-threaded** |
| [LangChain: How and when to build multi-agent systems](https://blog.langchain.com/how-and-when-to-build-multi-agent-systems) | Reconciles the Anthropic and Cognition posts: reading tasks parallelise better than writing tasks |
| [humanlayer/12-factor-agents](https://github.com/humanlayer/12-factor-agents) (⭐ 27k) | Production principles, especially *Own your context window* and *Small, Focused Agents* |

## 6. 🧩 Skills and agents on GitHub to borrow from

From [obra/superpowers](https://github.com/obra/superpowers) (⭐ 301k). Read them as checklists:

| Skill | What it teaches |
| --- | --- |
| [dispatching-parallel-agents](https://github.com/obra/superpowers/tree/main/skills/dispatching-parallel-agents) | When and how to fan out independent problems to parallel agents |
| [subagent-driven-development](https://github.com/obra/superpowers/tree/main/skills/subagent-driven-development) | Run a plan by giving each task to a fresh subagent, with review between tasks |
| [writing-plans](https://github.com/obra/superpowers/tree/main/skills/writing-plans) / [executing-plans](https://github.com/obra/superpowers/tree/main/skills/executing-plans) | The plan the orchestrator splits up: small, verifiable tasks |
| [using-git-worktrees](https://github.com/obra/superpowers/tree/main/skills/using-git-worktrees) | Isolate parallel writers |
| [verification-before-completion](https://github.com/obra/superpowers/tree/main/skills/verification-before-completion) | The orchestrator's final check: evidence before claims |

Agent collections and orchestration frameworks:

| Repo | Why |
| --- | --- |
| [VoltAgent/awesome-claude-code-subagents](https://github.com/VoltAgent/awesome-claude-code-subagents) (⭐ 26k) | 100+ ready subagent definitions: good examples of focused `description` + `tools` |
| [wshobson/agents](https://github.com/wshobson/agents) (⭐ 41k) | Subagents and plugins organised into multi-agent workflows (sequential, parallel, review) |
| [Yeachan-Heo/oh-my-claudecode](https://github.com/Yeachan-Heo/oh-my-claudecode) (⭐ 40k) | Teams-first multi-agent orchestration layer for Claude Code |
| [ruvnet/ruflo](https://github.com/ruvnet/ruflo) (formerly `claude-flow`, ⭐ 78k) | Heavyweight swarm orchestration with memory. Interesting to study, but probably overkill for daily work |
| [langchain-ai/open_deep_research](https://github.com/langchain-ai/open_deep_research) (⭐ 13k) | Open-source deep-research agent. Its legacy supervisor–researcher version is a readable reference |

---

## 7. 📋 My checklist before fanning out

- [ ] **Is it really parallel?** List the sub-tasks and check that they don't share state or files
- [ ] **Smallest setup that works:** single session → subagents → team, in that order
- [ ] **Each brief is self-contained:** goal, context, scope, tools, done-when, return format
- [ ] **Writers are isolated:** separate files or separate worktrees; one writer per file
- [ ] **Tools are scoped:** reviewers and researchers are read-only
- [ ] **Model is right-sized:** cheap model for search, strong model for the orchestrator
- [ ] **Summaries are short** so worker output doesn't flood the orchestrator
- [ ] **The orchestrator verifies:** tests run and the diff is read before I'm told it's done
- [ ] **I check token use** afterwards and ask whether the speedup was worth it
- [ ] **A brief I reuse becomes a subagent** in `.claude/agents/`, and a repeated lesson becomes a skill here
