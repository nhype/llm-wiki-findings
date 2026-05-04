# AI Agent Harnesses — Community Implementations

A survey of open-source "agent harness" projects on GitHub — the layer of code, configs, skills, hooks, and orchestration that wraps a coding LLM (Claude Code, Codex, OpenClaw, Cursor, Hermes) and turns the model into a working agent.

Snapshot date: **2026-05-04**.

---

## The Core Idea

A model trained to perceive, reason, and act provides the *agency*. The **harness** provides everything else the agent needs to operate in a real environment:

```
Harness = Tools + Knowledge + Observation + Action Interfaces + Permissions
```

The community has converged on a shared vocabulary — *skills, hooks, subagents, plans, memory, MCP, guardrails* — but split sharply on **what level of the stack the harness should occupy**: a config pack on top of Claude Code, a workflow loop, an org-level orchestrator, or a from-scratch runtime.

This file maps that split.

---

## Ranked by Stars

| # | Repo | ⭐ | Created | Commits/day | Closed-issue % | Cluster |
|---|------|---|---------|-------------|----------------|---------|
| 1 | [affaan-m/everything-claude-code](https://github.com/affaan-m/everything-claude-code) | 172,797 | 2026-01-18 | 14.7 | 87.5% (505) | A — Skills/Config Pack |
| 2 | [paperclipai/paperclip](https://github.com/paperclipai/paperclip) | 62,080 | 2026-03-02 | 38.0 | 14.6% (1,550) | C — Org-Level Orchestrator |
| 3 | [shareAI-lab/learn-claude-code](https://github.com/shareAI-lab/learn-claude-code) | 57,966 | 2025-06-29 | 0.22 | 30.1% (103) | E — From-Scratch Runtime |
| 4 | [multica-ai/multica](https://github.com/multica-ai/multica) | 24,162 | 2026-01-13 | 25.1 | 59.7% (554) | C — Managed-Agents Platform |
| 5 | [HKUDS/OpenHarness](https://github.com/HKUDS/OpenHarness) | 11,825 | 2026-04-01 | 10.7 | 92.8% (111) | E — From-Scratch Runtime |
| 6 | [revfactory/harness](https://github.com/revfactory/harness) | 3,098 | 2026-03-26 | 0.69 | 40.0% (5) | D — Meta / Skill-Factory |
| 7 | [NousResearch/hermes-paperclip-adapter](https://github.com/NousResearch/hermes-paperclip-adapter) | 1,083 | 2026-03-11 | 0.26 | 5.0% (40) | F — Adapter |
| 8 | [VILA-Lab/Dive-into-Claude-Code](https://github.com/VILA-Lab/Dive-into-Claude-Code) | 970 | 2026-04-11 | 2.09 | 100% (4) | G — Survey/Analysis |
| 9 | [Chachamaru127/claude-code-harness](https://github.com/Chachamaru127/claude-code-harness) | 721 | 2025-12-12 | 7.28 | 89.3% (28) | B — Plan→Work→Review Loop |
| 10 | [anothervibecoder-s/claudecode-harness](https://github.com/anothervibecoder-s/claudecode-harness) | 188 | 2026-04-19 | 0.20 | n/a (0) | A — Skills/Config Pack |
| 11 | [fredruss/agent-paperclip](https://github.com/fredruss/agent-paperclip) | 27 | 2026-01-16 | 1.15 | n/a (0) | F — Companion |

*Closed-issue % = closed / (closed + open) issues; total issue count in parentheses. Commits/day = total commits ÷ days since repo creation.*

---

## Ranked by Commit Velocity

| # | Repo | Commits/day | Total commits | Why it's fast |
|---|------|-------------|---------------|---------------|
| 1 | paperclipai/paperclip | **38.0** | 2,394 | Greenfield product (server + UI + many adapters); 63 days of construction |
| 2 | multica-ai/multica | **25.1** | 2,780 | Full task-management platform with WebSocket runtime, multi-workspace, board UI |
| 3 | affaan-m/everything-claude-code | **14.7** | 1,553 | Daily-driver config; 12+ language ecosystems, npm packages, GitHub App |
| 4 | HKUDS/OpenHarness | **10.7** | 353 | Academic-team push; React+Ink TUI, 43 tools, 114 tests in 33 days |
| 5 | Chachamaru127/claude-code-harness | **7.28** | 1,041 | Tracks every Claude Code minor release (2.1.99 → 2.1.111) |

The top three are all engineering platforms with paid/cloud surfaces; the velocity is product-shop velocity, not config-tweak velocity.

---

## Ranked by Issue-Triage Health

Closed-issue % among repos with **≥20 total issues** (smaller-N repos excluded — too noisy):

| # | Repo | Closed / Total | Health signal |
|---|------|----------------|---------------|
| 1 | HKUDS/OpenHarness | 103 / 111 (**92.8%**) | Tight academic team, fresh repo, no backlog accreted yet |
| 2 | Chachamaru127/claude-code-harness | 25 / 28 (**89.3%**) | Solo maintainer staying on top |
| 3 | affaan-m/everything-claude-code | 442 / 505 (**87.5%**) | Healthy at scale — the only large repo that doesn't drown |
| 4 | multica-ai/multica | 331 / 554 (**59.7%**) | Backlog growing as adoption scales |
| 5 | shareAI-lab/learn-claude-code | 31 / 103 (**30.1%**) | Educational repo — issues function as a discussion forum |
| 6 | paperclipai/paperclip | 227 / 1,550 (**14.6%**) | **Drowning** — 38 commits/day shipping faster than triage |
| 7 | NousResearch/hermes-paperclip-adapter | 2 / 40 (**5.0%**) | Adapter held together by a thin maintainer cap |

The interesting outlier is **everything-claude-code**: 173K stars and 87% closed rate. That's the only project keeping triage discipline at platform scale.

---

## Clusters — How They Differ

Seven distinct architectural bets. The split isn't "which CLI agent" — it's **which layer of the stack the harness occupies**.

### A. Skills/Config Packs (L0 — drop-in dotfiles for an existing CLI agent)
The model and runtime are someone else's (Claude Code, Codex, Cursor). The harness is a curated bundle of `CLAUDE.md`, agents, skills, hooks, MCP servers, and rules.
- **affaan-m/everything-claude-code** (172,797⭐) — the genre-definer; ships across Claude Code, Codex, Cursor, OpenCode, Gemini; npm-installable shims, GitHub App
- **anothervibecoder-s/claudecode-harness** (188⭐) — minimal blueprint: context discipline, hub-and-spoke subagents, multi-model consensus reviews

### B. Workflow Loops (L1 — disciplined Plan→Work→Review cycle on top of one CLI agent)
A finite-state loop (plan → execute → review → memory) wrapped around Claude Code as a daemon.
- **Chachamaru127/claude-code-harness** (721⭐) — Go-native guardrail engine; tracks Claude Code release contracts (`PreCompact` hook, `monitors.json`, 1h cache TTL); plug-installable

### C. Multi-Agent Orchestration Platforms (L2 — many agents, one team, one dashboard)
Server + UI that manage a *fleet* of agents as if they were employees on a board. Bring your own CLI agent.
- **paperclipai/paperclip** (62,080⭐) — "*If OpenClaw is an employee, Paperclip is the company*"; org charts, budgets, governance, goal alignment
- **multica-ai/multica** (24,162⭐) — Linear-shaped: agents have profiles, get assigned issues, post comments, raise blockers; full task lifecycle over WebSocket

### D. Meta / Skill-Factory (L3 — a harness that generates other harnesses)
Doesn't run agents itself. Takes a domain description and emits an agent team plus the skills they use.
- **revfactory/harness** (3,098⭐) — picks from 6 team patterns (Pipeline, Fan-out/Fan-in, Expert Pool, Producer-Reviewer, Supervisor, Hierarchical Delegation); writes `.claude/agents/` and `.claude/skills/`

### E. From-Scratch Harness Runtimes (build the vehicle, not ride one)
Implement the whole agent loop — tool-use, planning, memory, multi-agent coordination — without depending on Claude Code or Codex as the host process.
- **shareAI-lab/learn-claude-code** (57,966⭐) — pedagogical: "*agency comes from the model, vehicle is the harness*"; Bash + nano-harness from zero
- **HKUDS/OpenHarness** (11,825⭐) — research-grade Python runtime with React+Ink TUI, 43 built-in tools, 114 passing tests; ships **ohmo**, a personal agent built on top

### F. Companions & Adapters (around-the-edges)
Not a harness — peripherals.
- **fredruss/agent-paperclip** (27⭐) — desktop "Clippy" that watches Claude Code/Codex sessions for status, token usage, and "needs input" prompts
- **NousResearch/hermes-paperclip-adapter** (1,083⭐) — wires the Hermes model into a Paperclip "company" as a managed employee

### G. Surveys / Analyses (not a tool)
- **VILA-Lab/Dive-into-Claude-Code** (970⭐) — academic dissection of Claude Code's design as a reference for future agent systems

---

## Patterns & Observations

- **The market clusters at the extremes.** Either you ship a config pack riding on Claude Code (cluster A — cheap, high-leverage; ECC has 173K stars) or a full orchestration platform (cluster C — capital-intensive but sticky; Paperclip ships 38 commits/day). The middle layers (B, D) have far fewer entrants.
- **Stars and shipping discipline are independent.** Paperclip leads commit velocity (38/day) but lags triage (14.6%). ECC leads stars (173K) and triage (87.5%). The Chachamaru harness has 1/240th the stars of ECC but the second-best triage rate. **Velocity ≠ health ≠ adoption.**
- **"Harness" is being claimed at four different layers** — and the projects know it. revfactory/harness explicitly publishes its own L0–L3 layer taxonomy in the README and names its neighbors (Archon, ECC, meta-harness). That kind of self-positioning is rare; most repos still pretend they're alone in the market.
- **CLI-agnostic is now table stakes.** Multica supports 11 CLI agents (Claude Code, Codex, Copilot CLI, OpenClaw, OpenCode, Hermes, Gemini, Pi, Cursor Agent, Kimi, Kiro). ECC supports 5+. Paperclip 6+. Single-CLI lock-in is a 2025 mindset.
- **"Multics, again."** Multica's name is a deliberate callback to the 1960s time-sharing OS — humans and agents multiplexing one workspace. The framing has stuck: agents-as-teammates (cluster C) is replacing agents-as-tools as the dominant product metaphor.
- **Educational repos are quietly enormous.** `shareAI-lab/learn-claude-code` has 58K stars on **0.22 commits/day** — built once in mid-2025, barely touched since. The thesis ("agency comes from the model, not the orchestration code") is what people are starring, not the code.
- **Two repos called `everything-claude-code` exist.** affaan-m/* has 172K stars; worldflowai/* has 29 and was last touched on its creation day. Name collision in a hot category — buyer beware.
