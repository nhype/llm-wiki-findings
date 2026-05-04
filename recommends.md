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

## TL;DR

| Use case | Stack |
|----------|-------|
| **Telegram bot maintenance** | Claude Code + ECC + Archon + lean-ctx (+ GitNexus if codebase is large) |
| **Crypto opportunity scouting** | Hermes Agent *or* Multica (4 named roles) + DefiLlama/Dune/Etherscan MCPs + lean-ctx — **research-only**, with you as the execution gate and a hardware wallet for signing |

**The single most important rule: never give an agent your private keys.**
