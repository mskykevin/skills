# Git + AI agent best practices

Reference links for two things this library cares about:

1. **Git and GitHub skills:** courses, cheat sheets, and conventions.
2. **Working well with an AI agent day to day:** official guides plus popular community
   Agent Skills worth reading or borrowing from when writing my own (e.g. `engineering/git-conventions`).

This is a reading list, not a skill: it has no `SKILL.md`, so the install script skips it.

Star counts as of Oct 2026.

---

## 1. 🌱 Learn Git & GitHub (start here)

| Resource | What it is | Why |
| --- | --- | --- |
| [skills/introduction-to-github](https://github.com/skills/introduction-to-github) | Official GitHub course (⭐ 11k) | Branches, commits, PRs in under an hour. Runs inside GitHub, no install. |
| [skills/review-pull-requests](https://github.com/skills/review-pull-requests) | Official GitHub course | Learn to review and comment on PRs |
| [skills/resolve-merge-conflicts](https://github.com/skills/resolve-merge-conflicts) | Official GitHub course | Merge conflicts are the #1 beginner scare. Practice one safely. |
| [skills/communicate-using-markdown](https://github.com/skills/communicate-using-markdown) | Official GitHub course | Write good READMEs and notes (like this file!) |
| [skills/hello-github-actions](https://github.com/skills/hello-github-actions) | Official GitHub course | Your first automation that runs on GitHub |
| [pcottle/learnGitBranching](https://github.com/pcottle/learnGitBranching) ([play it](https://learngitbranching.js.org)) | Interactive game (⭐ 34k) | *See* branches and merges visually. Best way to "get" Git. |
| [Pro Git book](https://git-scm.com/book/en/v2) | Free official book | The reference. Read chapters 1–3 first. |

## 2. 🆘 Git cheat sheets for when things go wrong

| Resource | Why |
| --- | --- |
| [k88hudson/git-flight-rules](https://github.com/k88hudson/git-flight-rules) (⭐ 43k) | "I did X, how do I undo it?" Answers for almost every Git mistake. |
| [git-tips/tips](https://github.com/git-tips/tips) (⭐ 22k) | Most-used Git commands and tricks |
| [Oh Shit, Git!?!](https://ohshitgit.com) | Plain-English fixes for common Git disasters |

## 3. ✅ Git best practices (conventions)

| Resource | Why |
| --- | --- |
| [Conventional Commits](https://www.conventionalcommits.org) ([repo](https://github.com/conventional-commits/conventionalcommits.org)) | Standard commit message format: `feat: …`, `fix: …`, `docs: …` |
| [carloscuesta/gitmoji](https://github.com/carloscuesta/gitmoji) (⭐ 17k) | A fun emoji guide for commit messages |
| [github/gitignore](https://github.com/github/gitignore) (⭐ 176k) | Ready-made `.gitignore` files so you never commit junk or secrets |

**My Git checklist:**
- [ ] One branch per task; never work straight on `main` in shared repos
- [ ] Small commits with clear messages ("Add X", "Fix Y")
- [ ] `git status` and `git diff` before every commit, so I know what I'm saving
- [ ] Never commit passwords, API keys, or `.env` files
- [ ] Pull before I start, push when I'm done

---

## 4. 🤖 Best practices for using an AI agent in daily work

### Official guides (read these first)

| Guide | Why |
| --- | --- |
| [Claude Code: best practices](https://www.anthropic.com/engineering/claude-code-best-practices) | Anthropic's own tips: give context, plan first, review the output |
| [Claude Code docs: best practices](https://code.claude.com/docs/en/best-practices) | Up-to-date version in the official docs |
| [Claude Code docs: common workflows](https://code.claude.com/docs/en/common-workflows) | Step-by-step recipes: fix a bug, write tests, make a PR |
| [Claude Code docs: skills](https://code.claude.com/docs/en/skills) | How Agent Skills work and how to write your own |
| [agentsmd/agents.md](https://github.com/agentsmd/agents.md) ([site](https://agents.md)) (⭐ 25k) | `AGENTS.md`: an open format for a "README for agents" that tells them how your repo works (Claude Code uses `CLAUDE.md` for the same idea) |

### Agent Skills for Git workflows

From [obra/superpowers](https://github.com/obra/superpowers) (⭐ 301k), the most popular agent skills library.
Read them like checklists, even if you never install them:

| Skill | What it teaches |
| --- | --- |
| [using-git-worktrees](https://github.com/obra/superpowers/tree/main/skills/using-git-worktrees) | Give each task its own isolated workspace |
| [finishing-a-development-branch](https://github.com/obra/superpowers/tree/main/skills/finishing-a-development-branch) | What to check before merging, then how to merge, open a PR, or clean up |
| [requesting-code-review](https://github.com/obra/superpowers/tree/main/skills/requesting-code-review) | Get your work reviewed before merging |
| [receiving-code-review](https://github.com/obra/superpowers/tree/main/skills/receiving-code-review) | Handle review feedback well |
| [verification-before-completion](https://github.com/obra/superpowers/tree/main/skills/verification-before-completion) | "Evidence before claims": run the checks before saying it's done |
| [systematic-debugging](https://github.com/obra/superpowers/tree/main/skills/systematic-debugging) | Find the root cause instead of guessing |

### Skill collections to browse

| Repo | Why |
| --- | --- |
| [anthropics/skills](https://github.com/anthropics/skills) (⭐ 182k) | Official examples. Start with [skill-creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator) to make your own. |
| [ComposioHQ/awesome-claude-skills](https://github.com/ComposioHQ/awesome-claude-skills) (⭐ 77k) | Big curated list of skills for daily work |
| [hesreallyhim/awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) (⭐ 56k) | Best Claude Code skills, commands, and workflows |
| [travisvn/awesome-claude-skills](https://github.com/travisvn/awesome-claude-skills) (⭐ 15k) | Another curated list, with good explanations |
| [wshobson/agents](https://github.com/wshobson/agents) (⭐ 41k) | Ready-made agents and plugins for Claude Code, Codex, Cursor, Copilot |
| [VoltAgent/awesome-claude-code-subagents](https://github.com/VoltAgent/awesome-claude-code-subagents) (⭐ 26k) | 100+ specialized "helper" agents |
| [github/awesome-copilot](https://github.com/github/awesome-copilot) (⭐ 40k) | Same idea for GitHub Copilot users |
| [anthropics/claude-code-action](https://github.com/anthropics/claude-code-action) (⭐ 10k) | Run Claude on your GitHub issues and PRs automatically |

### GitHub courses on AI agents

| Course | Why |
| --- | --- |
| [skills/getting-started-with-github-copilot](https://github.com/skills/getting-started-with-github-copilot) | Hands-on intro to coding with an AI pair programmer |
| [skills/expand-your-team-with-copilot](https://github.com/skills/expand-your-team-with-copilot) | Let an AI agent work on GitHub issues for you |

### My daily checklist when working with an agent

Lessons learned so far:

- [ ] **Say exactly what I want**, including where: which repo, folder, and branch
- [ ] **Ask for a plan first** on anything bigger than a small edit
- [ ] **Read the diff** before anything is committed or pushed. The agent can make mistakes.
- [ ] **Check commit authorship**: my contributions should be credited to *me*
- [ ] **Never paste secrets** (passwords, API keys) into a chat
- [ ] **Do the key steps myself sometimes** (fork, open the PR) so I actually learn them
- [ ] **Turn repeated corrections into a skill** in this repo
