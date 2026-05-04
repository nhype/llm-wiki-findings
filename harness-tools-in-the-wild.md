# Harness Tools in the Wild — Per-Tool Cases & Reported Effects

For each tool catalogued in [`harness-ai.md`](./harness-ai.md), what people are actually doing with it and what they report getting out of it.

Snapshot date: **2026-05-04**.

Numbers below are reported by users / authors in public sources (READMEs, blog posts, podcasts, X threads, DEV.to, Substack). Treat single-user "I saved X tokens" claims as anecdotes; treat author-published benchmarks as marketing-tinted but useful as ceilings.

---

## Cluster A — Skills / Config Packs

### affaan-m/everything-claude-code (172,797⭐)
**Who's using it:** ~102K stargazers, the de-facto starting point for "what does a serious Claude Code config look like." Affaan Mustafa won the Anthropic × Forum Ventures hackathon in Sept 2025 building zenith.chat using Claude Code, then released his entire setup as ECC.

**Reported effects:**
- A "fact-forcing gate" pattern in ECC measurably improves agent output by **+2.25 points** vs ungated baselines (author benchmark)
- ECC GitHub App is at "a few hundred in MRR" at ~**1% free-to-paid conversion** on the open-source repo — small in absolute terms but a working monetization signal at this layer
- Affaan reports the moment that mattered most: seeing ECC recommended on developer Discords by people who didn't know him. Word-of-mouth product/market fit at the config-pack layer

**Bundle size:** 13 agents + 40+ skills + hooks + MCPs across Claude Code, Codex, Cursor, OpenCode, Gemini.

### anothervibecoder-s/claudecode-harness (188⭐)
**Who's using it:** anonymous solo-founder author who runs Claude Code on Opus 4.7 for **8+ hours/day on SaaS projects** without hitting quota. Inspired by the viral r/ClaudeAI / r/claude / r/vibecoding discussion thread.

**Reported effects (author):** zero quota hits even on 5× Max plan with heavy tool use; "zero hallucinated success" via mandatory verification blocks; senior-level autonomy (model stops asking repetitive questions). No third-party metrics yet.

---

## Cluster B — Workflow Loops & Builders

### coleam00/Archon (20,680⭐)
**Who's using it:** teams looking for the "Dockerfile for AI coding workflows" — YAML-defined plan/implement/test/review/PR-create loops, committed to `.archon/workflows/` and shared across the team. Cole Medin completely rewrote Archon in TypeScript on **April 7, 2026**, archiving the Python codebase.

**Reported effects (front-page result):**
- Same LLM, same task: PR acceptance rate jumped from **6.7% → ~70%** when wrapped in a structured Archon harness — the single most-cited harness-engineering data point in this survey
- 17K stars with **+452 in a single day** post-rewrite; runs from CLI / Web / Slack / Telegram / GitHub / Discord
- `.archon/config.yaml` injects team coding standards into every agent run, standardizing output across team members

### Chachamaru127/claude-code-harness (721⭐)
**Who's using it:** solo + small-team Claude Code users wanting a Plan → Parallel Implementation → Review → Commit loop with strict guardrails. Tracks every Claude Code minor release contract — `PreCompact` hook (CC 2.1.105), `monitors.json` plugin manifest, 1-hour cache opt-in (CC 2.1.108), `xhigh` review effort (CC 2.1.111 + Opus 4.7).

**Reported effects:** maintainer publishes per-release migration tables (e.g., "v4.1 → v4.2: PreCompact blocks compaction while a worker is active") with regression test counts (17 new tests for CC 2.1.110 conformance). Less third-party data than Archon, but the maintainer's release discipline is unusually high.

---

## Cluster C — Multi-Agent Orchestration Platforms

### paperclipai/paperclip (62,080⭐)
**Who's using it:** the orchestration layer beneath several of the documented zero-human-company experiments (see [`autonomous-businesses.md`](./autonomous-businesses.md)). Pseudonymous founder @dotta launched March 4, 2026; **30K stars in 3 weeks**.

**Reported effects:**
- Used as the "company OS" wrapper around OpenClaw / Hermes / Claude Code for solo founders building "agent companies"
- Most credible deployment trace: **NousResearch/hermes-paperclip-adapter** — published wiring to run Hermes as a managed employee inside a Paperclip company
- The catch — backlog discipline collapses at this scale: **14.6% closed-issue rate on 1,550 issues** (per `harness-ai.md`). Velocity ≠ stability

### ruvnet/ruflo (ex–claude-flow) (39,425⭐)
**Who's using it:** approximately **100,000 monthly active users across 80+ countries** (project-reported). Used for multi-agent swarm orchestration on top of Claude Code.

**Reported effects:**
- v3.5.0 was the **first stable release after 10 months, 5,800+ commits, and 55 alpha iterations** — meaning the prior several months of users were running alpha software in production
- Coordinates **60+ specialized agents** with **215 MCP tools**; self-learning memory + fault-tolerant consensus
- Project rebranded from claude-flow → Ruflo as part of v3.x rebuild

### multica-ai/multica (24,162⭐)
**Who's using it:** small teams (2 humans + N agents) wanting Linear-shaped task management where AI agents claim issues from a queue. Hit **#1 on GitHub TypeScript Trending in April 2026**, accumulated **10.7K stars in 3 months**.

**Reported effects:** the "skill compounding" model — when one agent solves an infrastructure issue, the solution is packaged into a reusable skill for all subsequent agents. No public per-team productivity numbers yet, but the model is being copied across the cluster (paperclip, ruflo).

---

## Cluster D — Meta / Skill-Factory

### revfactory/harness (3,098⭐)
**Who's using it:** Korean and Japanese Claude Code communities heavily — README ships in EN/KO/JA simultaneously (`하네스 구성해줘`, `ハーネスを構成して`). Companion repo **`revfactory/harness-100`** ships **100 production-ready agent-team harnesses across 10 domains in EN+KO** (200 packages total), each with 4–5 specialist agents + orchestrator skill + 2–3 extending skills.

**Reported effects:** "Harness Evolution Mechanism" captures the delta between the initial generated architecture and what was actually shipped, feeding it back so the next harness in a similar domain starts closer to shipped state. No published productivity benchmarks yet — strength is in the breadth of pre-built domain templates.

### stanford-iris-lab/meta-harness (770⭐)
**Who's using it:** the academic eval community. This is the reference implementation for the *Meta-Harness* paper.

**Reported effects (audited benchmark, not anecdote):**
- **76.4% on Terminal-Bench 2.0 with Claude Opus 4.6** — beat hand-engineered Terminus-KIRA (74.7%) and ranked **#2** on the public TerminalBench-2 leaderboard
- The trick: environment bootstrapping — gather a snapshot of the sandbox environment and inject it into the initial prompt. Saves **2–5 exploration turns** the agent normally spends on `ls`, `which python3`, etc. Tiny change, top-2 result

### SaehwanPark/meta-harness (29⭐)
**Who's using it:** Codex users wanting the same six team-architecture patterns as `revfactory/harness`. Direct port — same six patterns (Pipeline, Fan-out/Fan-in, Expert Pool, Producer-Reviewer, Supervisor, Hierarchical Delegation), Codex runtime instead of Claude Code. No public deployment data; included for completeness.

---

## Cluster E — From-Scratch Runtimes & Standalone Agents

### openclaw/openclaw (367,991⭐)
**Who's using it:** the largest user base of any tool in this survey. **Felix** (Nat Eliason) generated $80K–$300K in revenue running on OpenClaw. **Ron** (Robby Houston) hit $8.4K MRR in 13 days from $100 starting capital. Beyond those headline cases, Redwerk surveyed 100+ active users tracking community projects through early 2026.

**Reported effects from documented users:**
- OpenClaw-assisted blog posts rank in **4–6 weeks** vs **8–12 weeks** for fully manual content (Redwerk cohort)
- 25+ documented production use cases catalogued at TLDL
- 90% closed-issue rate on **33,643 total issues** — astonishing triage discipline at this scale (only large repo in the survey not buried in backlog)
- See [`autonomous-businesses.md`](./autonomous-businesses.md) for the full Felix and HeyRon stack writeups

### NousResearch/hermes-agent (131,507⭐)
**Who's using it:** users running "personal AI agents that live on a server and talk to you through messaging apps."

**Documented user stories from the project's own user-stories page:**
- One user: **autoresearch + Karpathy LLM-wiki second-brain + skills creation + scheduled jobs + background monitoring**, accessed via Telegram and Discord
- One user: **two Telegram groups through one gateway** — a project group and a horse-racing community
- One developer: **production software-development pipeline — 3-actor email processing with DBOS + PostgreSQL + S3 + Gmail API**, running daily
- Clawdi team quote: *"Hermes is the best self-improving agent we've used — it gets smarter the longer you run it. The WhatsApp and Telegram integrations make it feel genuinely personal."*

**Caveat:** **only 36.6% closed-issue rate on 4,622 issues** — high-visibility ⇒ flood of low-effort issues, only 1/3 triaged.

### shareAI-lab/learn-claude-code (57,966⭐)
**Who's using it:** ~58K stars on **0.22 commits/day** — barely touched since mid-2025. The repo is consumed as **reading material**, not run as software. The thesis being starred: *"agency comes from the model, not the orchestration code."*

**Reported effect:** spawned a wave of "build your own mini Claude Code" tutorials (e.g., truongpx396 on DEV.to: *"Learn harness engineering by building a mini Claude Code"*). The most-cited on-ramp into harness engineering as a discipline.

### HKUDS/OpenHarness (11,825⭐)
**Who's using it:** primarily HKUDS team + early adopters via **ohmo**, the bundled personal agent (`oh` CLI launches the runtime). Ohmo runs on existing Claude Code or Codex subscriptions — no extra API key.

**Reported effects:**
- v0.1.6 by April 10, 2026 (still pre-1.0); 43 built-in tools, 114 passing tests, React+Ink TUI
- Notable production-hardening event: **PR #127** addressed a remote-message-driven slash-command vulnerability that could change permission mode and trigger arbitrary file reads. Signals the project is being run by enough people to surface real security issues
- 92.8% closed-issue rate (highest tier) — small academic team staying on top of triage

---

## Cluster F — Token-Optimization / Context Layer

### rtk-ai/rtk (40,797⭐)
**Who's using it:** Claude Code / Cursor / Copilot / Gemini / Aider / Codex users running long sessions. The most-deployed tool in this cluster.

**Reported numbers (real users, not author):**
- One developer: **10M tokens saved (89% reduction)** on Claude Code sessions in a single shared writeup that went viral on Reddit / GitHub Discussions / Hacker News
- One sustained user: **15,720 commands processed, 138M tokens saved at 88.9% efficiency** after a few weeks of daily use
- Author benchmarks across 2,900+ real commands: **average 89% compression**

**Per-command examples:**
| Command | Before | After | Compression |
|---------|--------|-------|-------------|
| `cargo test` (262 passing) | 4,823 tok | 11 tok | **99%** |
| `git diff HEAD~1` (large change) | 21,500 tok | 1,259 tok | 94% |
| `cat src/main.rs` (1,295 lines) | 10,176 tok | 504 tok | 95% |

### mksglu/context-mode (12,377⭐)
**Who's using it:** Mert Köseoğlu's project; deployed across 14 platforms via MCP. The benchmark in the README: **315 KB of raw tool output reduced to 5.4 KB — 98% reduction**.

**Reported user impact:**
- *"Sessions that used to hit the wall at 30 minutes now run for 3 hours. The same 200K tokens, used more carefully."*
- Stack: 11 MCP tools — 6 sandbox (`ctx_batch_execute`, `ctx_execute`, `ctx_execute_file`, `ctx_index`, `ctx_search`, `ctx_fetch_and_index`) + 5 meta (`ctx_stats`, `ctx_doctor`, `ctx_upgrade`, `ctx_purge`, `ctx_insight`)
- Each execute call spawns an isolated subprocess; only stdout enters the conversation

### yvgude/lean-ctx (1,053⭐)
**Who's using it:** smaller user base than RTK or context-mode but the **highest reported per-user dollar savings** in any testimonial collected.

**Real user quotes:**
- *"`lean-ctx gain` shows I've saved **$48 in one week**. That's real money."*
- *"Session caching alone — re-reads cost **13 tokens instead of thousands**."*
- *"Shell hook saves **hundreds of thousands of tokens per day**. `git log`, `cargo test`, `npm run` — everything compressed automatically. Zero config."*
- Skeptic-converted: *"I was skeptical about the 99% claim. Then I ran `ctx_read` in map mode on a 400-line service file and got back **30 tokens**."*

**Comparison from a user who ran both:** *"lean-ctx is a completely different level — it doesn't just compress output, it understands code structure."* (vs prior token-proxy tool, unspecified which)

### edouard-claude/snip (204⭐)
**Who's using it:** Go-language partisans wanting RTK-equivalent compression without Rust. Same value proposition (60–90% reduction via declarative YAML filters), much smaller user base.

**Reported effect:** essentially RTK's claims at smaller scale; no independent benchmarks published yet.

---

## Cluster G — Code-Knowledge-Graph Backends

### safishamsi/graphify (41,981⭐)
**Who's using it:** users on **14+ harnesses** — Claude Code, Codex, OpenCode, Cursor, Gemini CLI, Copilot CLI, Aider, OpenClaw, Factory Droid, Trae, Hermes, Kiro, Pi, Google Antigravity.

**Headline reported result:**
- 3 GPT-framework repos + 5 attention papers + 4 diagrams = **52 files / 92K words → 285 nodes, 340 edges, 53 communities**. Average query cost: **~1.7K tokens vs ~123K naive — 71.5× reduction.**
- Multi-modal: code, PDFs, markdown, screenshots, diagrams, whiteboard photos, images, audio. **Videos transcribed with Whisper using a domain-aware prompt** derived from your corpus

### abhigyanpatwari/GitNexus (35,302⭐)
**Who's using it:** the closest thing to a "code-graph standard" for MCP-enabled coding agents. The README's framing: a *"senior dev sitting next to you, pointing at a whiteboard."*

**Reported effects:**
- `gitnexus analyze --skills` auto-generates a `SKILL.md` per detected functional area — describing key files, entry points, execution flows, cross-area connections
- 7 MCP tools + 2 guided prompts; `detect_changes` for pre-commit risk analysis; `rename` for coordinated multi-file symbol renames; `generate_map` for auto-Mermaid architecture diagrams
- Trending growth — featured on TrendShift, ranked global #894 by stars
- Privacy claim that's part of the appeal: **runs entirely client-side / locally** — no code leaves the machine

---

## Cluster H — Deployment & Adapter Tooling

### openclaw/acpx (2,538⭐)
**Who's using it:** **other agents and orchestrators**, not humans directly. The README is explicit: *"the primary user is another agent, orchestrator, or harness."* The smallest useful Agent Client Protocol (ACP) client; provides one CLI surface for Pi, OpenClaw, Codex, Claude.

**Reported effects:** 98.4% closed-issue rate on a tight scope. Used in the Multica / Paperclip / OpenClaw + VS Code wiring tutorials as the agent-to-agent transport. No deployment metrics — it's plumbing.

### chrysb/alphaclaw (1,259⭐)
**Who's using it:** OpenClaw users who don't want to manage the gateway themselves. By Chrys Bader; ships Railway / Render templates (one-click deploy) plus a GUI over OpenClaw's CLI.

**Real user quotes:**
- *"AlphaClaw took the server headache away completely."*
- *"I never touch the terminal or OpenClaw UI. Now have **7 agents running across products**. Game changer."*
- Operator features being praised: **automatic hourly commits** of workspace to GitHub (every agent action versioned and auditable); **watchdog** that detects crashes, enters repair mode, relaunches gateway, notifies owner

**Anti-lock-in stance:** *"AlphaClaw is not a proprietary dependency — remove it and your agent keeps running, with nothing proprietary to migrate."* This is rare in the cluster and likely a deliberate trust-building move.

### NousResearch/hermes-paperclip-adapter (1,083⭐)
**Who's using it:** the small intersection of "I run Hermes Agent" ∩ "I run a Paperclip company." Wires Hermes in as a managed employee inside a Paperclip org structure.

**Reported state:** 5.0% closed-issue rate on 40 issues — hobby-grade adoption. Important as a **proof point** that two different "standalone agent" + "company OS" projects compose, not as a workhorse production tool.

### fredruss/agent-paperclip (27⭐)
**Who's using it:** Claude Code / Codex users tired of staring at the terminal. Desktop "Clippy"-style monitor showing status, token usage, and "needs input" prompts.

**Reported effect:** quality-of-life improvement, no productivity claims.

---

## Cluster I — Surveys & Awesome Lists

### ai-boost/awesome-harness-engineering (709⭐)
**Who's using it:** practitioners building agents who want a starting bibliography. Curates Anthropic's context-engineering guide, OpenAI's eval guide, OpenHands evals playbook, **VoltAgent's catalogue of 363+ arXiv papers** organized into 5 harness-relevant categories (Multi-Agent: 51, Memory & RAG: 56, Eval & Observability: 79, Agent Tooling: 95, AI Agent Security: 82), plus benchmarks (WebArena-Verified, WildClawBench, WorkArena).

**Reported effect:** the index, not the artifact — value is in saving the search.

### VILA-Lab/Dive-into-Claude-Code (970⭐)
**Who's using it:** academics. It's a paper, not a runtime. *"A Systematic Analysis and Discussion of Claude Code for Designing Today's and Future AI Agent Systems."* 100% issue-closure rate (4 issues, all closed) is the entire production signal you'll get from a survey paper.

---

## What These Cases Add Up To

A few patterns visible only when the per-tool cases are stacked side-by-side:

1. **The biggest reported productivity wins come from the *cheapest* tools, not the biggest platforms.** Archon (21K⭐) reports a **6.7% → 70%** PR-acceptance jump from one structural change. Stanford's Meta-Harness (770⭐) hits **76.4%** on Terminal-Bench 2 by injecting an environment snapshot. Neither requires changing the model.

2. **Token-proxy users speak in dollar figures, not percentages.** lean-ctx's *"$48 saved in one week"* and RTK's *"138M tokens saved"* are the only places in this whole survey where users report savings in concrete money. That's because token-proxy ROI is measurable per-session — orchestration ROI isn't.

3. **The "named agent" pattern keeps recurring.** Felix, Ron, Claudius, Iris, Remy, Ohmo. Tools that anchor to a named persistent identity (OpenClaw, Hermes, Claudius) get used for businesses; tools that don't (most of cluster A and B) get used for *workflow*. Naming may be more architecturally load-bearing than people realize.

4. **The Korean/Japanese/Chinese harness ecosystem is real and largely invisible to English-language searches.** revfactory/harness ships in 3 languages day-one; Multica, Chachamaru/claude-code-harness, shareAI-lab/learn-claude-code all have major Chinese/Japanese READMEs. The earlier `harness-ai.md` v1 missed this almost entirely.

5. **Issue-closure rate is the single best signal of production-readiness** — better than stars or commits/day. The four tools with >97% closure (lean-ctx, acpx, context-mode, snip) are all mature in their narrow scope. The two tools <15% (Paperclip 14.6%, Hermes-Paperclip-Adapter 5%) are either over-extended or hobby-grade.

6. **The supply chain is starting to compose.** Felix runs on OpenClaw + RTK-class compression + Sentry. AlphaClaw deploys OpenClaw. Hermes-Paperclip-Adapter wires Hermes into Paperclip. revfactory/harness publishes its own L0–L3 layer taxonomy naming Archon and ECC as neighbors. **Two years in, the harness layer has named neighbors and supply chains** — that's the strongest signal that this is no longer an experiment phase.
