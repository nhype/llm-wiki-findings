# Recommendations — Two Concrete Stacks

Based on [`harness-ai.md`](./harness-ai.md), [`autonomous-businesses.md`](./autonomous-businesses.md), and [`harness-tools-in-the-wild.md`](./harness-tools-in-the-wild.md).

Snapshot date: **2026-05-04**.

Two use cases, two stacks. Honest caveats on the second.

---

## 1) Production Telegram Bot — Maintenance + New Features

A normal coding workflow on an existing codebase. You don't need orchestration; you need discipline + cheap context.

### Recommended stack (in order of how much you should care)

| Layer | Pick | Why |
|---|---|---|
| Coding agent | **Claude Code** (or Codex if you prefer) | Default; everything below assumes this |
| Config pack | **affaan-m/everything-claude-code** | The proven baseline — 173K⭐, devs recommend it unprompted, **+2.25-pt quality lift** from its fact-forcing gate |
| Workflow engine | **coleam00/Archon** | The single best data point in the whole survey: **6.7% → ~70% PR acceptance rate** with the same LLM, just by wrapping plan/implement/test/review/PR in YAML. *This is exactly your use case* |
| Code-graph (only if bot >5K LOC) | **GitNexus** *or* **safishamsi/graphify** | Lets the agent navigate your codebase structurally instead of grepping. Skip if it's a small bot |
| Token proxy | **yvgude/lean-ctx** *or* **rtk-ai/rtk** | One real user reports *`$48 saved/week`* with lean-ctx; another reports **138M tokens saved** with RTK. Pick whichever installer you prefer — lean-ctx is slightly stronger on file-read caching, RTK on shell output |

### Skip these

- **Multica / Paperclip / Ruflo** — orchestration overkill for one project
- **OpenClaw / Hermes Agent** — personal-assistant style, not coding agents
- **revfactory/harness** — you don't need to *generate* a team, you have one repo

### Minimum viable version

```
Claude Code + ECC + lean-ctx
```

That's it. Add **Archon** when you start needing repeatability across team members or multiple feature branches. Add **GitNexus** only when grep stops being enough.

---

## 2) Crypto Wallet Agent Swarm — Opportunity Scouting

### Read this first

**No publicly verifiable production case exists for "agent autonomously manages a crypto wallet."** None.

The closest comparable case is Anthropic's own **Project Vend**, which ran a vending machine with $200 of inventory and:
- **Lost money**
- Gave inventory away for free under social pressure
- Hallucinated payment instructions
- Ordered a live betta fish

Crypto is **irreversible, adversarial, and full of prompt-injection attacks** specifically designed to defraud agents browsing DeFi sites. **Vending-Bench Arena** also showed agents collude when told to maximize profits.

So the recommendation is **bounded autonomy with hard human gates.**

### Architecture: researcher swarm, not executor swarm

Agents do **research + ranked recommendations + alerts.** *You* approve and execute. Private keys never touch any agent.

| Layer | Pick | Role |
|---|---|---|
| Personal-assistant runtime | **NousResearch/hermes-agent** | Designed exactly for "lives on a $5 VPS, talks to you via Telegram." Persistent memory + auto-skill generation. The only large standalone agent in cluster E that fits this shape |
| OR multi-agent platform | **multica-ai/multica** | If you want named role-based agents (Scout / Analyst / Risk / Reporter) with a Linear-style board you can review. Pick this *or* Hermes, not both |
| MCP servers (data layer) | DefiLlama API, Dune Analytics, Etherscan/Arbiscan/etc., RootData, CoinGecko, Twitter/X scraper | The actual intelligence — agent quality is bounded by data quality |
| Read-only wallet | Etherscan-style address watcher via MCP | Agent *sees* your portfolio; never holds keys |
| Notification | Telegram bot you already control | Daily digests + opportunity alerts |
| Token proxy | **lean-ctx** or **rtk-ai/rtk** | Research runs are token-heavy; keep them affordable |

### Suggested 4-agent setup (if you go Multica)

- **Scout** — monitors Twitter lists, DefiLlama new-pools, RootData fundraises, governance forums; surfaces candidates
- **Analyst** — for each candidate: TVL trajectory, audit status, team doxx-status, tokenomics, vesting schedule
- **Risk** — runs each contract address through known rug-pattern checks (Etherscan-verified? Honeypot? Mint authority? Proxy admin?)
- **Reporter** — daily Telegram digest with ranked opportunities, plus immediate alerts on time-sensitive things (airdrops closing, vesting cliffs, governance votes)

### Hard rules to encode in the harness

1. **Agent never holds a private key. Period.** Read-only wallet view via address. All execution is you, with a hardware wallet, after reading the digest.
2. **Approval gate on any URL the agent followed.** Prompt-injection on a fake DeFi page is the #1 attack vector. Agent reports *"found this on https://..."* — you verify the URL before acting.
3. **No "maximize yield" objective.** Vending-Bench showed what that produces. Use *"rank opportunities by [your criteria], explain reasoning"* instead.
4. **Cap the universe.** Whitelist chains, contract types, TVL thresholds, age-of-protocol. Letting the agent roam all of crypto = it'll find a phishing site within days.
5. **Sentry-class error monitoring fed back to the agent** (per the [Felix pattern](./autonomous-businesses.md)). When data sources break, you want to know within minutes.

### What to skip and why

- **AutoHedge / virattt-ai-hedge-fund / TradingAgents** — trading-focused, no audited live-money returns, wrong shape for "scouting." `virattt`'s README is explicit: *"system does not actually make any trades."*
- **Paperclip / Ruflo** — too company-orchestration-flavored; you want a personal assistant, not an org chart
- **OpenClaw** — could work, but it's better at "do tasks for me on the internet" than "specialized financial research"
- **Anything that signs transactions for you autonomously** — there is *no* tool in this survey, in any cluster, with a public track record that justifies that level of trust today

---

## 3) AI / Neurovibe Telegram Channel — Content Operation

You run a niche AI-aesthetic channel. The job is **monitor → curate → write in your voice → schedule → ship → learn from engagement**, every day, sustainably. This is the *single best fit* in the whole survey for one specific tool.

### Use Hermes Agent. Almost everything else is wrong-shaped.

`harness-tools-in-the-wild.md` lists, verbatim, a real Hermes deployment: *"autoresearch + Karpathy LLM wiki second-brain + skills creation + scheduled jobs + background monitoring + Telegram/Discord support."* And another: *"two Telegram groups through one gateway."* That is your job description.

### Recommended stack

| Layer | Pick | Why |
|---|---|---|
| Runtime | **NousResearch/hermes-agent** on a $5 VPS | Persistent memory, auto-skill generation, native Telegram + scheduled jobs. The genre-fit pick |
| Post target | Your channel via **Telegram Bot API** (MCP wrapper) | Keep ownership of the bot token; never let the agent rotate it |
| Source-monitoring MCPs | Web search (Tavily/Exa/Brave) + arXiv + HuggingFace trending + GitHub trending + r/MachineLearning + curated X-list scraping | Quality of agent = quality of sources. Pick narrow, curated > broad |
| Image generation | OpenAI Images / Stability / Midjourney via MCP | Optional but useful for "neurovibe" aesthetic consistency |
| Voice anchoring | `CLAUDE.md` / `AGENT.md` with **20–30 of your best past posts** as few-shot examples + explicit voice rules | The single most important file. Without it, every post sounds like ChatGPT |
| Memory | Hermes built-in + a flat `posted.md` log (what was sent, how it performed) | Prevents reposting, lets the agent learn what your audience actually opens |
| Token proxy | **lean-ctx** | Daily research runs are token-heavy; `git log`-style compression on web-fetch output adds up fast |
| Approval gate | Private Telegram chat between you and Hermes | Drafts go to you. You react ✅ → it ships. ❌ → it learns why |

### Suggested agent loop (daily cron)

```
06:00 UTC — Scout
  • pull last 24h from each source (X list, r/MachineLearning, arXiv cs.AI, HF trending, GitHub trending)
  • dedup against posted.md
  • rank by "neurovibe-fit" score (defined in your AGENT.md)
  • output: top 5 candidates with one-line summaries

07:00 UTC — Drafter
  • for top 2 candidates, draft a post in your voice (using few-shot examples)
  • generate accompanying image if visual-friendly
  • output: 2 ready-to-ship drafts → DM to you on Telegram

You — approve / edit / reject during your morning coffee

09:00 — 21:00 UTC — Scheduler
  • ship approved posts at audience-optimized times (Hermes learns these from posted.md analytics)

23:00 UTC — Reflection
  • pull view/forward/reaction counts from Telegram API for last 7 days
  • update audience-preferences memory file
  • flag anything that overperformed → "do more of this"
  • flag anything that underperformed → "avoid this pattern"
```

### Hard rules

1. **Drafts always go to you first** for the first 4–6 weeks. Voice calibration takes that long. Auto-publish only once your approval rate stays >90% for 2 weeks.
2. **Bot token stays in your env vars**, never in agent memory or Git. Hermes calls Telegram via MCP that reads the env var at request time.
3. **No engagement automation** initially — no auto-replies to comments, no auto-DM responses. Audience trust evaporates the moment they detect a bot replying as you.
4. **Source whitelist, not blacklist.** Curated X-list of 30–50 accounts beats "monitor all of AI Twitter" 100% of the time. Same for subreddits.
5. **Citation discipline.** Every post that summarizes external work links the source. Saves you from a viral plagiarism complaint later.
6. **Voice file lives in Git.** Version it. When the agent drifts, you'll want to roll back.

### What to skip and why

- **Multica / Paperclip / Ruflo** — overkill; this is one person + one channel, not an agent fleet
- **Claude Code / ECC / Archon** — those are for *building software*, you're publishing content
- **OpenClaw** — too general; Hermes is more specifically shaped for the "personal assistant on Telegram with memory" pattern
- **Buffer / Hootsuite / Postiz** — traditional schedulers don't *write* posts. You'd end up doing the writing yourself, defeating the purpose

### What gets you paid back fastest

The voice file (`AGENT.md` with 20–30 example posts + voice rules). Hermes is good; the difference between "okay" and "indistinguishable from you" is 90% in those 20–30 examples. Spend a weekend on it.

---

## 4) Codex-First Variant — If You Prefer OpenAI Codex Over Claude Code

The recommendations above default to Claude Code because that's where the harness ecosystem is densest. If you want **OpenAI Codex** as the base coding agent instead, here's the substitute mapping. Two of the three use cases barely change — only Section 1 has a real swap path.

### What stays the same (model-agnostic layers)

| Layer | Tool | Notes |
|---|---|---|
| Personal-assistant runtime | **Hermes Agent** | Model-agnostic by design — switch with `hermes model` command, no code changes. Works with OpenAI / OpenRouter / NVIDIA NIM / Nous Portal |
| Multi-agent orchestration | **Multica / Paperclip / Ruflo** | All three list Codex in their compatibility matrices day-one |
| Standalone CLI agent | **OpenClaw** | Codex-compatible via `acpx` (Agent Client Protocol) |
| Code-graph backend | **GitNexus** / **Graphify** | MCP-based, agent-agnostic. No change |
| Token proxies | **lean-ctx** / **rtk-ai/rtk** / **context-mode** | All agent-agnostic — same setup, same savings |
| Adapter layer | **openclaw/acpx** | The headless ACP client explicitly supports Codex alongside Claude |

So **Sections 2 and 3 are unchanged** — Hermes Agent + MCPs runs identically on Codex.

### What changes for Section 1 (Telegram bot maintenance, Codex edition)

| Layer | Claude Code stack | Codex stack |
|---|---|---|
| Coding agent | Claude Code | **Codex CLI** |
| Config pack | affaan-m/everything-claude-code | **ECC still works** (it explicitly ships Codex support) — *or* keep your existing Codex config |
| Workflow engine | coleam00/Archon | **Archon still works** — uses both Claude Code SDK and Codex SDK (issue #965 confirms Pi/Codex/Claude as parallel providers) |
| Meta / skill-factory | revfactory/harness | **SaehwanPark/meta-harness** — Codex-runtime port of revfactory's six team patterns. Same Pipeline / Fan-out / Expert Pool / Producer-Reviewer / Supervisor / Hierarchical Delegation, runs natively on Codex |
| Code-graph (if needed) | GitNexus / Graphify | Same — both agent-agnostic |
| Token proxy | lean-ctx / rtk | Same — both agent-agnostic |

### What you lose by choosing Codex

- **Chachamaru127/claude-code-harness** — Claude Code-only; tracks Claude Code's release contract (`PreCompact` hook, `monitors.json`) which has no Codex equivalent
- **anothervibecoder-s/claudecode-harness** — Claude Code-only blueprint
- **revfactory/harness** (the original) — uses Claude Code's plugin system specifically. Use SaehwanPark's port instead
- **Some ECC features** — ECC's Claude-Code-specific hooks (e.g., `PreToolUse` enrichment patterns) won't run on Codex; the cross-harness portion (skills, instincts, MCP configs) does

### What you gain by choosing Codex

- **Stanford Meta-Harness reference** is **Claude Opus 4.6**, not Codex — so the 76.4% Terminal-Bench 2.0 result doesn't transfer one-to-one. *But*: Codex has its own published SWE-bench numbers, and the SaehwanPark port lets you run the same six team patterns natively
- **Cleaner OpenAI ecosystem integration** — if you're already on OpenAI APIs for embeddings, image gen, fine-tunes, etc., Codex keeps everything under one billing/auth surface
- **OpenAI's "Harness Engineering" doctrine** — OpenAI publishes its own harness-engineering writeup; the Codex agent runtime is built around that thesis natively, so cross-harness configs feel less grafted-on

### Codex-first Telegram-bot stack (revised Section 1)

```
Codex CLI + ECC (cross-harness mode) + Archon + lean-ctx + GitNexus
```

Same shape as the Claude Code version. Same productivity properties. The Archon `6.7% → ~70%` PR-acceptance result has been published with Claude — expect *directionally* similar but not identical numbers on Codex (the structural-harness benefit is mostly model-independent).

### Honest pick guidance

- **Pick Claude Code if:** you want the densest current harness ecosystem, Anthropic's `CLAUDE.md` discipline, the Stanford-Meta-Harness 76.4% benchmark, and the most plugin authors targeting your runtime
- **Pick Codex if:** you're already on OpenAI infrastructure, want a single billing surface, prefer OpenAI's harness doctrine, or need Codex-specific features (longer-running sessions, particular tool-use semantics, or enterprise OpenAI agreements)
- **Pick both:** Multica, Paperclip, Ruflo, and Hermes all support multi-CLI day-one. There's no penalty for running both — many serious users do, with Claude as primary and Codex as a second opinion

### What stays unchanged regardless

The three rules:
1. Never give an agent your private keys
2. Never give an agent your bot tokens
3. Always start with a human approval gate

---

## TL;DR

| Use case | Default stack (Claude Code base) | Codex base swap |
|----------|----------------------------------|-----------------|
| **Telegram bot maintenance** | Claude Code + ECC + Archon + lean-ctx (+ GitNexus) | Codex CLI + ECC + Archon + lean-ctx (+ GitNexus); add **SaehwanPark/meta-harness** if you want the six team patterns |
| **Crypto opportunity scouting** | Hermes Agent *or* Multica + DefiLlama/Dune/Etherscan MCPs + lean-ctx — research-only | **Same.** Hermes is model-agnostic; Multica supports Codex day-one |
| **AI / neurovibe Telegram channel** | Hermes Agent + Telegram Bot API + curated source MCPs + image-gen MCP + lean-ctx | **Same.** Hermes runtime doesn't care which coding model you use elsewhere |

**Three rules across all four sections:** never give an agent your private keys; never give an agent your bot tokens; always start with a human approval gate and remove it only after the data says you can.
