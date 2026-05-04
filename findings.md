# Karpathy's LLM Wiki — Community Implementations

Findings from the gist [karpathy/442a6bf555914893e9891c11519de94f](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) and the projects shared in its comments.

Snapshot date: **2026-05-04**.

---

## The Core Idea

Karpathy proposes replacing per-query RAG with a **persistent, compounding markdown wiki** that an LLM maintains incrementally. Three layers:

1. **Raw sources** — immutable input documents (PDFs, code, URLs, notes).
2. **The wiki** — LLM-generated markdown: summaries, entity pages, concept pages with cross-links.
3. **The schema** — config that directs the LLM's compile/update workflow.

Key shift: synthesis happens **once, at ingestion**, not on every query. Cross-references are pre-computed; the artifact grows richer over time.

---

## Most Popular Implementations (by stars)

| # | Repo | ⭐ | Created | Angle |
|---|------|---|---------|-------|
| 1 | [Lum1104/Understand-Anything](https://github.com/Lum1104/Understand-Anything) | 10,935 | 2026-03-15 | Interactive knowledge graph from any code/wiki |
| 2 | [AgriciDaniel/claude-obsidian](https://github.com/AgriciDaniel/claude-obsidian) | 4,105 | 2026-04-07 | Claude + Obsidian companion (`/wiki /save /autoresearch`) |
| 3 | [sdyckjq-lab/llm-wiki-skill](https://github.com/sdyckjq-lab/llm-wiki-skill) | 1,276 | 2026-04-05 | Multi-platform Claude Skill |
| 4 | [atomicmemory/llm-wiki-compiler](https://github.com/atomicmemory/llm-wiki-compiler) | 973 | 2026-04-05 | Raw sources → interlinked wiki |
| 5 | [Ar9av/obsidian-wiki](https://github.com/Ar9av/obsidian-wiki) | 912 | 2026-04-06 | Agent framework for Obsidian |
| 6 | [lucasastorian/llmwiki](https://github.com/lucasastorian/llmwiki) | 793 | 2026-04-04 | Open-source canonical impl with MCP |
| 7 | [Astro-Han/karpathy-llm-wiki](https://github.com/Astro-Han/karpathy-llm-wiki) | 725 | 2026-04-05 | Skills-compatible (Claude/Cursor/Codex) |
| 8 | [skyllwt/OmegaWiki](https://github.com/skyllwt/OmegaWiki) | 472 | 2026-04-09 | Research platform, 23 Claude skills |
| 9 | [kytmanov/obsidian-llm-wiki-local](https://github.com/kytmanov/obsidian-llm-wiki-local) | 464 | 2026-04-07 | 100% local via Ollama |
| 10 | [swarmclawai/swarmvault](https://github.com/swarmclawai/swarmvault) | 351 | 2026-04-06 | 48-agent MCP integration |
| 11 | [NicholasSpisak/second-brain](https://github.com/NicholasSpisak/second-brain) | 268 | 2026-04-06 | Obsidian second-brain |
| 12 | [ussumant/llm-wiki-compiler](https://github.com/ussumant/llm-wiki-compiler) | 241 | 2026-04-04 | Claude Code plugin |
| 13 | [Ansub/wiki-os](https://github.com/Ansub/wiki-os) | 222 | 2026-04-12 | UI layer for the wiki |
| 14 | [Pratiyush/llm-wiki](https://github.com/Pratiyush/llm-wiki) | 217 | 2026-04-08 | Captures multi-CLI sessions |
| 15 | [mduongvandinh/llm-wiki](https://github.com/mduongvandinh/llm-wiki) | 208 | 2026-04-07 | Vietnamese localization |
| 16 | [ojuschugh1/sqz](https://github.com/ojuschugh1/sqz) | 184 | 2026-04-12 | Token compression (adjacent) |
| 17 | [doum1004/llmwiki-cli](https://github.com/doum1004/llmwiki-cli) | 73 | 2026-04-10 | TS CLI, multi-wiki |

---

## Most New & Noteworthy

Repos created in the last ~2 weeks that are differentiated despite low star counts. These are the leading edge of the pattern.

| Repo | ⭐ | Created | Why notable |
|------|---|---------|-------------|
| [VeniVeci/VLM-wiki](https://github.com/VeniVeci/VLM-wiki) | 3 | **2026-05-03** | First **multimodal** extension — images, video, audio with bidirectional linking |
| [cianmarbo/Lore](https://github.com/cianmarbo/Lore) | 1 | **2026-05-02** | Wiki built **from Claude Code conversations** mid-session |
| [hanyuancheung/llm-skill](https://github.com/hanyuancheung/llm-skill) | 7 | 2026-05-01 | Generalizes the pattern to **skills** (execute/distill/guide layers), not just knowledge |
| [jgoldfed/keppi](https://github.com/jgoldfed/keppi) | 5 | 2026-04-30 | **Blast-radius queries** + gap detection over weighted directed graph |
| [ojuschugh1/Aura](https://github.com/ojuschugh1/Aura) | 4 | 2026-04-26 | Local AI daemon with **claim verification** + MCP observability |
| [radimsem/remindb](https://github.com/radimsem/remindb) | 10 | 2026-04-15 | **Single SQLite file** as portable agent memory; 75–99% token cut |
| [Ansub/wiki-os](https://github.com/Ansub/wiki-os) | 222 | 2026-04-12 | **Fastest grower in UI space** — pure presentation layer, agnostic to compiler |
| [ojuschugh1/sqz](https://github.com/ojuschugh1/sqz) | 184 | 2026-04-12 | **Strong stars-per-day** (Rust); reframes wiki as token-compression cache |

---

## Clusters — How They Differ

Eight axes of differentiation emerged. Most projects sit in 1–2 clusters.

### 1. Obsidian-native (use Obsidian as the UI)
The biggest single design choice. Authors lean on Obsidian's existing graph view + markdown linking.
- **AgriciDaniel/claude-obsidian** (4,105⭐) — slash-command UX
- **Ar9av/obsidian-wiki** (912⭐) — agent framework
- **kytmanov/obsidian-llm-wiki-local** (464⭐) — distinguished by **100% local with Ollama**, no cloud
- **NicholasSpisak/second-brain** (268⭐)
- **jgoldfed/keppi** (5⭐) — query layer over Obsidian vaults

### 2. Knowledge-graph / Visualization
Treat the wiki as a graph first, markdown second. Focus is on browsing and relationships.
- **Lum1104/Understand-Anything** (10,935⭐) — by far the most popular; "graphs that teach > graphs that impress"
- **Ansub/wiki-os** (222⭐) — UI-only, pluggable into any compiler
- **jgoldfed/keppi** (5⭐) — weighted directed graphs, blast-radius queries

### 3. Knowledge Compilers (sources → wiki pipeline)
The most literal reading of Karpathy's idea. Emphasize ingestion + compilation.
- **atomicmemory/llm-wiki-compiler** (973⭐)
- **ussumant/llm-wiki-compiler** (241⭐)
- **tuirk/Kompl** (6⭐) — multiformat (URLs, files, bookmarks), MCP integration
- **cagataysengor/llm-wiki-studio** (2⭐) — productized RAG

### 4. Claude Skills / Multi-CLI Integration
Package the pattern as a Skill or MCP server callable from Claude Code, Cursor, Codex, Gemini, etc.
- **sdyckjq-lab/llm-wiki-skill** (1,276⭐) — Chinese-led, multi-platform
- **Astro-Han/karpathy-llm-wiki** (725⭐)
- **skyllwt/OmegaWiki** (472⭐) — 23 skills covering full research lifecycle
- **swarmclawai/swarmvault** (351⭐) — **48 agent integrations**
- **lucasastorian/llmwiki** (793⭐) — canonical MCP server impl
- **gowtham0992/link** (24⭐) — local MCP wiki viewer
- **hanyuancheung/llm-skill** (7⭐) — generalizes pattern beyond knowledge to skills

### 5. Session-Capture (LLM conversations → wiki)
Inverts the input: instead of you feeding sources, the LLM session itself is the source.
- **Pratiyush/llm-wiki** (217⭐) — captures Claude Code/Codex/Cursor/Gemini sessions
- **cianmarbo/Lore** (1⭐) — mid-session, automatic, topic-linked

### 6. Memory Infrastructure (agent-first, not user-first)
Prioritize the agent's persistent memory; the wiki is a side effect.
- **momhq/mom** (41⭐) — lifecycle tracking, scope-based ACLs
- **radimsem/remindb** (10⭐) — SQLite-backed, portable
- **ojuschugh1/sqz** (184⭐) — pure token compression, caches frequent reads
- **ojuschugh1/Aura** (4⭐) — daemon + claim verification

### 7. CLI / Local-First
Headless, scriptable, no UI assumptions.
- **doum1004/llmwiki-cli** (73⭐) — multi-wiki support
- **kytmanov/obsidian-llm-wiki-local** (464⭐) — overlaps Obsidian cluster, but local Ollama is the headline

### 8. Multimodal & Niche
- **VeniVeci/VLM-wiki** (3⭐) — extends pattern to images/video/audio (the only one)
- **mduongvandinh/llm-wiki** (208⭐) — Vietnamese localization
- **multilingualprogramming/multilingual** (2⭐) — knowledge as programming primitive
- **Fly-Carrot/shared-fabric-codex-gemini** (2⭐) — bioacoustics-research-specific

---

## Patterns & Observations

- **Implementation explosion in early April 2026**, ~2–3 weeks after the gist. Most top repos were created within 4 days of each other (Apr 4–9).
- **Two design poles**: "wiki for humans" (Obsidian, UI) vs "memory for agents" (mom, remindb, sqz). Karpathy's gist reads more like the former; the agent-memory cluster is a community pivot.
- **MCP is the dominant integration**, not custom UIs. ~10 of the listed projects ship MCP servers.
- **Graph view is winning the visualization war**: the #1 project (10,935⭐) is graph-first, not document-first.
- **The newest projects (May 2026) are differentiating sideways** — multimodal, session-capture, claim-verification, blast-radius — rather than re-implementing the core compiler. Saturation in the center, exploration at the edges.
