# AI Agent Harnesses — Community Implementations

A survey of open-source "agent harness" projects on GitHub — the layer of code, configs, skills, hooks, and orchestration that wraps a coding LLM (Claude Code, Codex, OpenClaw, Cursor, Hermes) and turns the model into a working agent.

Snapshot date: **2026-05-04**.

---

## Revision Note (v2)

The first pass of this file (commit `895085e`) covered 11 repos and missed entire categories of harness tooling. Specifically: **standalone CLI agents** (OpenClaw, Hermes Agent), **token-optimization proxies** (RTK, lean-ctx), **code-knowledge-graph backends** (GitNexus, Graphify), and **workflow-engine harness builders** (Archon).

Why the misses, in order of severity:

1. **Anchored on three example names.** I searched literally for the names supplied in the prompt ("multica, paperclip, everythingclaudecode") and close variants instead of fanning out across the harness ecosystem.
2. **Scoped "harness" too narrowly** — only as wrapper / orchestrator / runtime. Left out the *support stack*: token proxies, code-graph backends, standalone agent models. These are harness *components*, not adjacent.
3. **Failed to introspect.** RTK was already named in my own session config; GitNexus MCP tools were in my own tool list; Hermes was name-checked by two repos I had already read. Three of the missing entries were sitting in my context window.

v2 covers **22 repos** across **9 clusters**, including the four categories above.

---

## The Core Idea

A model trained to perceive, reason, and act provides the *agency*. The **harness** provides everything else the agent needs to operate in a real environment:

```
Harness = Tools + Knowledge + Observation + Action Interfaces + Permissions
```

The community has converged on shared vocabulary — *skills, hooks, subagents, plans, memory, MCP, guardrails, context discipline* — but split sharply on **what level of the stack the harness should occupy**: the agent itself, the runtime, the workflow loop, the orchestration platform, the context layer, or the code-graph backend.

---

## Ranked by Stars

| # | Repo | ⭐ | Created | Commits/day | Closed-issue % | Cluster |
|---|------|---|---------|-------------|----------------|---------|
| 1 | [openclaw/openclaw](https://github.com/openclaw/openclaw) | 367,991 | 2025-11-24 | **254.3** | 90.0% (33,643) | E — Standalone CLI Agent |
| 2 | [affaan-m/everything-claude-code](https://github.com/affaan-m/everything-claude-code) | 172,797 | 2026-01-18 | 14.7 | 87.5% (505) | A — Skills/Config Pack |
| 3 | [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent) | 131,507 | 2025-07-22 | 24.5 | 36.6% (4,622) | E — Standalone CLI Agent |
| 4 | [paperclipai/paperclip](https://github.com/paperclipai/paperclip) | 62,080 | 2026-03-02 | 38.0 | 14.6% (1,550) | C — Org-Level Orchestrator |
| 5 | [shareAI-lab/learn-claude-code](https://github.com/shareAI-lab/learn-claude-code) | 57,966 | 2025-06-29 | 0.22 | 30.1% (103) | E — From-Scratch Runtime |
| 6 | [safishamsi/graphify](https://github.com/safishamsi/graphify) | 41,981 | 2026-04-03 | 9.6 | 76.1% (331) | G — Code-Graph Backend |
| 7 | [rtk-ai/rtk](https://github.com/rtk-ai/rtk) | 40,797 | 2026-01-22 | 8.5 | 38.5% (675) | F — Token-Optimization Proxy |
| 8 | [ruvnet/ruflo](https://github.com/ruvnet/ruflo) (ex-claude-flow) | 39,425 | 2025-06-02 | 18.3 | 61.7% (1,051) | C — Multi-Agent Swarm |
| 9 | [abhigyanpatwari/GitNexus](https://github.com/abhigyanpatwari/GitNexus) | 35,302 | 2025-08-02 | 3.0 | 56.4% (551) | G — Code-Graph Backend |
| 10 | [multica-ai/multica](https://github.com/multica-ai/multica) | 24,162 | 2026-01-13 | 25.1 | 59.7% (554) | C — Managed-Agents Platform |
| 11 | [coleam00/Archon](https://github.com/coleam00/Archon) | 20,680 | 2025-02-07 | 3.0 | 76.0% (633) | B — Workflow Engine |
| 12 | [mksglu/context-mode](https://github.com/mksglu/context-mode) | 12,377 | 2026-02-23 | 16.2 | 97.1% (175) | F — Token-Optimization Proxy |
| 13 | [HKUDS/OpenHarness](https://github.com/HKUDS/OpenHarness) | 11,825 | 2026-04-01 | 10.7 | 92.8% (111) | E — From-Scratch Runtime |
| 14 | [revfactory/harness](https://github.com/revfactory/harness) | 3,098 | 2026-03-26 | 0.69 | 40.0% (5) | D — Meta / Skill-Factory |
| 15 | [openclaw/acpx](https://github.com/openclaw/acpx) | 2,538 | 2026-02-17 | 4.3 | 98.4% (61) | H — Adapter / ACP Client |
| 16 | [chrysb/alphaclaw](https://github.com/chrysb/alphaclaw) | 1,259 | 2026-02-25 | 7.7 | 75.0% (24) | H — Deployment Harness |
| 17 | [NousResearch/hermes-paperclip-adapter](https://github.com/NousResearch/hermes-paperclip-adapter) | 1,083 | 2026-03-11 | 0.26 | 5.0% (40) | H — Adapter |
| 18 | [yvgude/lean-ctx](https://github.com/yvgude/lean-ctx) | 1,053 | 2026-03-23 | **28.5** | **99.2%** (124) | F — Token-Optimization Proxy |
| 19 | [VILA-Lab/Dive-into-Claude-Code](https://github.com/VILA-Lab/Dive-into-Claude-Code) | 970 | 2026-04-11 | 2.09 | 100% (4) | I — Survey |
| 20 | [stanford-iris-lab/meta-harness](https://github.com/stanford-iris-lab/meta-harness) | 770 | 2026-04-15 | 0.58 | 50% (4) | D — Meta / Skill-Factory |
| 21 | [Chachamaru127/claude-code-harness](https://github.com/Chachamaru127/claude-code-harness) | 721 | 2025-12-12 | 7.3 | 89.3% (28) | B — Plan→Work→Review Loop |
| 22 | [ai-boost/awesome-harness-engineering](https://github.com/ai-boost/awesome-harness-engineering) | 709 | 2026-03-29 | — | — | I — Awesome List |

*Closed-issue % = closed / (closed + open) issues; total in parentheses. Commits/day = total commits ÷ days since creation.*

Smaller-but-noteworthy entries discussed in clusters: `edouard-claude/snip` (204⭐), `anothervibecoder-s/claudecode-harness` (188⭐), `SaehwanPark/meta-harness` (29⭐), `fredruss/agent-paperclip` (27⭐).

---

## Ranked by Commit Velocity

| # | Repo | Commits/day | Total | Why it's fast |
|---|------|-------------|-------|---------------|
| 1 | openclaw/openclaw | **254.3** | 40,941 | A real CLI agent under heavy active development; 161 days, 75K forks |
| 2 | paperclipai/paperclip | 38.0 | 2,394 | Greenfield org-orchestrator; many adapters in parallel |
| 3 | yvgude/lean-ctx | 28.5 | 1,199 | Aggressive single-author Rust runtime; 90+ filter patterns |
| 4 | multica-ai/multica | 25.1 | 2,780 | Full task-management platform with WebSocket runtime |
| 5 | NousResearch/hermes-agent | 24.5 | 7,008 | Self-improving agent with auto-skill generation |
| 6 | ruvnet/ruflo | 18.3 | 6,138 | Continuous v3.x rebuild for swarm orchestration |
| 7 | mksglu/context-mode | 16.2 | 1,131 | Context-window optimizer for 14 platforms |
| 8 | affaan-m/everything-claude-code | 14.7 | 1,553 | Daily-driver config across 12+ language ecosystems |
| 9 | HKUDS/OpenHarness | 10.7 | 353 | Academic team push; React+Ink TUI, 43 tools |
| 10 | safishamsi/graphify | 9.6 | 298 | Tree-sitter + LLM hybrid graph builder |

OpenClaw's velocity is in a different regime — 254 commits/day means roughly 10 commits per hour, sustained over 5 months. That's not a single-author cadence; it's a coordinated agent ecosystem.

---

## Ranked by Issue-Triage Health

Closed-issue % among repos with **≥20 total issues**:

| # | Repo | Closed / Total | Health signal |
|---|------|----------------|---------------|
| 1 | yvgude/lean-ctx | 123 / 124 (**99.2%**) | New project, tight maintainer, no backlog |
| 2 | openclaw/acpx | 60 / 61 (**98.4%**) | Narrow scope (ACP client) — easy to triage |
| 3 | mksglu/context-mode | 170 / 175 (**97.1%**) | Disciplined Turkish team; aggressive scope control |
| 4 | edouard-claude/snip | 22 / 23 (**95.7%**) | Niche (Go alternative to RTK), limited surface |
| 5 | HKUDS/OpenHarness | 103 / 111 (**92.8%**) | Academic team, fresh repo |
| 6 | openclaw/openclaw | 30,284 / 33,643 (**90.0%**) | **Astonishing** — 90% triage at 33K issues |
| 7 | Chachamaru127/claude-code-harness | 25 / 28 (**89.3%**) | Solo maintainer staying on top |
| 8 | affaan-m/everything-claude-code | 442 / 505 (**87.5%**) | Healthy at config-pack scale |
| 9 | safishamsi/graphify | 252 / 331 (**76.1%**) | Hot 31-day-old repo, still triaging well |
| 10 | coleam00/Archon | 481 / 633 (**76.0%**) | Long-lived, well-funded community |
| 11 | chrysb/alphaclaw | 18 / 24 (**75.0%**) | Tight scope (OpenClaw deployer) |
| 12 | ruvnet/ruflo | 648 / 1,051 (**61.7%**) | Active rebuild churn — backlog accreting |
| 13 | multica-ai/multica | 331 / 554 (**59.7%**) | Backlog growing as adoption scales |
| 14 | abhigyanpatwari/GitNexus | 311 / 551 (**56.4%**) | Mid-tier triage; client-side complexity |
| 15 | rtk-ai/rtk | 260 / 675 (**38.5%**) | Backlog from rapid pattern requests |
| 16 | NousResearch/hermes-agent | 1,692 / 4,622 (**36.6%**) | High visibility ⇒ flood; only 36% triaged |
| 17 | shareAI-lab/learn-claude-code | 31 / 103 (**30.1%**) | Educational repo — issues = forum |
| 18 | paperclipai/paperclip | 227 / 1,550 (**14.6%**) | **Drowning** — shipping faster than triaging |
| 19 | NousResearch/hermes-paperclip-adapter | 2 / 40 (**5.0%**) | Hobby adapter, barely maintained |

---

## Clusters — How They Differ

Nine architectural bets across the harness stack. Read this section as the punchline of the survey: it's *which layer of the stack you're building at* that matters, not which CLI agent you target.

### A. Skills / Config Packs (L0 — drop-in dotfiles for an existing CLI agent)
Curated bundle of `CLAUDE.md`, agents, skills, hooks, MCP servers, rules. Ride on top of someone else's runtime.
- **affaan-m/everything-claude-code** (172,797⭐) — genre-definer; ships across Claude Code, Codex, Cursor, OpenCode, Gemini
- **anothervibecoder-s/claudecode-harness** (188⭐) — minimal blueprint: context discipline + hub/spoke + multi-model consensus

### B. Workflow Loops & Builders (L1 — disciplined cycles + the engines that author them)
A finite-state loop (plan → execute → review → memory) wrapped around a CLI agent, plus the YAML/DSL engines that let you define such loops repeatably.
- **coleam00/Archon** (20,680⭐) — *"the first open-source harness builder"*; YAML workflow engine for plan/implement/validate/review/PR-create
- **Chachamaru127/claude-code-harness** (721⭐) — Go-native guardrail engine; tracks Claude Code's `PreCompact` hook contract release-by-release

### C. Multi-Agent Orchestration Platforms (L2 — fleet of agents, one team, one dashboard)
Server + UI managing many agents as if they were employees. Bring your own CLI agent.
- **paperclipai/paperclip** (62,080⭐) — *"if OpenClaw is an employee, Paperclip is the company"* — org charts, budgets, governance
- **ruvnet/ruflo** (39,425⭐, ex-claude-flow) — swarm coordinator with RAG, federation, distributed swarm intelligence
- **multica-ai/multica** (24,162⭐) — Linear-shaped: agents have profiles, get assigned issues, post comments, raise blockers

### D. Meta / Skill-Factory (L3 — a harness that generates other harnesses)
Doesn't run agents itself. Takes a domain description and emits an agent team plus the skills they use.
- **revfactory/harness** (3,098⭐) — 6 team patterns (Pipeline, Fan-out/Fan-in, Expert Pool, Producer-Reviewer, Supervisor, Hierarchical Delegation); writes `.claude/agents/` and `.claude/skills/`
- **stanford-iris-lab/meta-harness** (770⭐) — reference code for the *Meta-Harness* paper; achieves **76.4% on Terminal-Bench 2.0** with Claude Opus 4.6
- **SaehwanPark/meta-harness** (29⭐) — Codex-runtime port of revfactory/harness's concept

### E. From-Scratch Runtimes & Standalone Agents
Implement the agent loop without depending on Claude Code/Codex as the host process. The four entries here split into two sub-types:

**Standalone CLI agents (the actual product):**
- **openclaw/openclaw** (367,991⭐) — *"your own personal AI assistant, any OS, any platform, the lobster way"*; the most-starred coding agent on GitHub; multi-channel inbox (WhatsApp, Telegram, Slack, Discord, +); local-first Gateway
- **NousResearch/hermes-agent** (131,507⭐) — *"the agent that grows with you"*; auto-generates skills from experience; runs on $5 VPS or GPU cluster; model-agnostic (Nous Portal, OpenRouter, NVIDIA NIM, OpenAI)

**Educational / research runtimes (the explanation):**
- **shareAI-lab/learn-claude-code** (57,966⭐) — *"agency comes from the model, vehicle is the harness"*; nano-harness from zero in Bash
- **HKUDS/OpenHarness** (11,825⭐) — Python runtime with React+Ink TUI, 43 built-in tools, 114 passing tests; ships **ohmo** as the bundled personal agent

### F. Token-Optimization / Context Layer (NEW in v2)
CLI proxies and shell hooks that compress tool output before it reaches the LLM. Often Rust binaries with declarative pattern files. **The most-overlooked harness category — and the one with the loudest performance claims.**
- **rtk-ai/rtk** (40,797⭐) — *Rust Token Killer*; CLI proxy filtering terminal output; **89% average compression**; works with 12 AI coding tools
- **mksglu/context-mode** (12,377⭐) — context-window optimization across 14 platforms; sandboxes tool output; claims 98% reduction
- **yvgude/lean-ctx** (1,053⭐) — context layer with shell hook + MCP server; 49 tools, 10 read modes, 90+ patterns; 99% on cached reads
- **edouard-claude/snip** (204⭐) — Go alternative to RTK; declarative YAML filters

### G. Code-Knowledge-Graph Backends (NEW in v2)
Index a repo into a structured graph (calls, imports, inheritance, execution flows), then expose the graph to coding agents over MCP. Replaces text-search + vector RAG with structural queries.
- **safishamsi/graphify** (41,981⭐) — Tree-sitter static analysis + LLM-driven semantic extraction; turns code, SQL schemas, R scripts, docs, papers, images into one queryable graph
- **abhigyanpatwari/GitNexus** (35,302⭐) — MCP-native; pre-computes dependency structure at index time; ships agent skills (Exploring, Debugging, Impact Analysis, Refactoring) plus PreToolUse / PostToolUse hooks

### H. Deployment & Adapter Tooling (around-the-edges)
- **openclaw/acpx** (2,538⭐) — headless ACP (Agent Client Protocol) client; one command surface for Pi, OpenClaw, Codex, Claude
- **chrysb/alphaclaw** (1,259⭐) — *"the ultimate setup harness for OpenClaw"*; deploy in minutes, run for months, no CLI required
- **NousResearch/hermes-paperclip-adapter** (1,083⭐) — wires Hermes into a Paperclip company as a managed employee
- **fredruss/agent-paperclip** (27⭐) — desktop "Clippy" that watches Claude Code/Codex sessions

### I. Surveys & Awesome Lists
- **ai-boost/awesome-harness-engineering** (709⭐) — curated list across tools, patterns, evals, memory, MCP, permissions, observability, orchestration
- **VILA-Lab/Dive-into-Claude-Code** (970⭐) — academic dissection of Claude Code's design

---

## Patterns & Observations

- **The actual top of the harness food chain is OpenClaw, not Claude Code derivatives.** OpenClaw has 368K stars, 90% closed-issue rate, and 254 commits/day. The *vast* majority of "Claude Code harness" tooling sits in clusters A–D and is dwarfed in stars and velocity by OpenClaw itself. Many CLI-agent-agnostic tools list OpenClaw first in their compatibility matrix.
- **Token-optimization is a distinct, hot category that I missed in v1.** Combined stars across cluster F: ~54K. The market has decided that *compressing terminal output* is a separate harness layer, not a feature of the agent. RTK alone outranks Multica + OpenHarness combined.
- **Code-graph backends are the second missed category.** GitNexus + Graphify = ~77K combined. They beat traditional RAG by replacing similarity search with *structural* queries (call graphs, inheritance, blast-radius). MCP is the universal delivery mechanism.
- **"Harness" is being claimed at six different layers of the stack** — agent (E), runtime (E), workflow loop (B), orchestration platform (C), config pack (A), and context proxy (F). revfactory/harness is the only project that publishes its own L0–L3 layer taxonomy and names its neighbors. Most repos still pretend the category is theirs alone.
- **Velocity, stars, and triage health are independent variables.** OpenClaw has all three. Paperclip has stars + velocity but is drowning in issues (14.6% triage). lean-ctx has 99.2% triage on 28.5 commits/day but only 1K stars. Optimize for whichever you actually need.
- **CLI-agnostic is now table stakes.** Multica supports 11 CLI agents; ECC supports 5+; Paperclip 6+; lean-ctx and RTK each support 6+. Lock-in to one CLI agent is a 2025 mindset.
- **"Multics, again."** Multica's name (Mul-tiplexed I-nformation and C-omputing A-gent) is a deliberate callback to the 1960s OS. The framing has stuck across the field: agents-as-teammates is replacing agents-as-tools as the dominant product metaphor, and OpenClaw's Gateway / Multica's board / Paperclip's org-chart are three takes on the same idea.
- **Educational repos quietly accumulate enormous stars.** `shareAI-lab/learn-claude-code` has 58K stars on **0.22 commits/day** — built once in mid-2025, barely touched since. The thesis ("agency comes from the model, not the orchestration code") is what people are starring, not the code itself.
- **Two repos called `everything-claude-code` exist; two called `meta-harness` exist.** `affaan-m/*` (173K) vs `worldflowai/*` (29); `revfactory/harness` (3K, Claude Code) vs `SaehwanPark/meta-harness` (29, Codex port) vs `stanford-iris-lab/meta-harness` (770, the Stanford Terminal-Bench paper). Naming collisions are a leading indicator of category heat.

---

## What This Survey Still Misses

Honesty up front so future revisions can fill the gaps:

- **Eval / benchmark harnesses** — SWE-bench, Terminal-Bench, agent-bench wrappers (only one entry: stanford-iris-lab/meta-harness)
- **Sandbox / sandboxing layers** — E2B, microVM-based agent sandboxes
- **Browser-agent harnesses** — Browser Use, Stagehand, Skyvern (this survey is coding-agent–biased)
- **Agent observability** — LangSmith-style tracing, replay, debugging tools
- **Locale-specific ecosystems** — most Chinese/Japanese/Korean harness repos surfaced here are translations; the native communities likely have more
