# Sovereign AIOS — Product Requirements Document (PRD)

**Version:** 1.1
**Author:** Trade
**Date:** 2026-04-30
**Status:** Planning — Ready for Phase 1

---

## 1. Product Overview

**Sovereign AIOS** is a self-hosted, zero-human-approval agentic operating system. It coordinates a team of specialized AI agents through a unified management UI, shared memory infrastructure, and a universal communication protocol (MCP). Designed to run 24/7 as a background OS — handling research, coding, monitoring, and task execution autonomously — with a human in the loop only for high-stakes decisions.

**Sovereign Files:** All configuration, directives, memory data, and brain files are owned locally. Nothing leaves the local infrastructure without client-side encryption.

**Core Thesis:** A team of purpose-built agents, sharing context via a common brain, outperforms any single model — and stays fully under user control.

---

## 2. Architecture

### 2.1 System Layers

| Layer | Component | Role |
|-------|-----------|------|
| **Management (UI)** | Multica | Linear-style task dashboard; assigns work, tracks PRs, monitors agent status |
| **Orchestration** | **Claude Code** | **Lead Orchestrator.** Breaks down high-level tasks into sub-tasks, manages complex coding workflows, spawns sub-agents via tmux/MCP |
| **Execution** | Hermes + Agent Zero | Hermes: fast code loops, analysis. Agent Zero: autonomous tool-building, complex coding |
| **Infrastructure (OS)** | OpenFang | Rust-based always-on OS; runs memory servers and background monitors 24/7 |
| **Hot Memory** | Honcho | Real-time session state, user preferences, agent social traits |
| **Deep Memory** | Open Brain | Semantic KB, long-term project facts, cross-agent knowledge sharing (pgvector) |
| **Protocol Layer** | MCP Bridge | Universal adapter; all components share tools, memory, and context via MCP |

### 2.2 Agent Roles

| Agent | Role | Best For | Docker | Windows | WSL/Linux |
|-------|------|----------|--------|---------|-----------|
| **Claude Code** | Lead Orchestrator + Senior Dev | Complex planning, architectural decisions, multi-step workflows, sub-agent spawning | ✅ | ✅ | ✅ |
| **Hermes** | Execution Expert | Fast code loops, data analysis, research, CLI gateway | ✅ | ✅ | ✅ |
| **Agent Zero** | Autonomous Dev | High-complexity coding, building its own tools | ✅ | ✅ | ✅ |
| **OpenFang** | Infrastructure OS | 24/7 background workers, bare-metal performance | ❌ | ⚠️ | ✅ |
| **Multica** | Project Manager UI | Task assignment, progress tracking | ✅ | ✅ | ✅ |
| **Honcho** | Hot Memory | Real-time user preferences, session state | ✅ | ✅ | ✅ |
| **Open Brain** | Deep Memory | Semantic KB, pgvector cross-agent knowledge | ✅ | ✅ | ✅ |

### 2.3 LLM Provider Strategy (4 Options)

| # | Provider | Use Case | Auth | Notes |
|---|---------|----------|------|-------|
| **1** | **OpenRouter** | Best overall: 100+ models, single API key, auto-failover | API key | Primary; Claude Sonnet 4, GPT-4o, DeepSeek, etc. |
| **2** | **Ollama Cloud** | Fast, cheap, simple tasks | API key | `https://ollama.com/v1` endpoint |
| **3** | **Claude OAuth** | Complex reasoning, code, analysis | Browser OAuth / API key | Anthropic direct; strongest coding agent |
| **4** | **OpenWebUI (Local)** | Privacy-sensitive, offline, sovereign data | No API key | Runs on localhost; fully offline capable |

**Provider Fallback Chain:** OpenRouter → Ollama Cloud → Claude OAuth → OpenWebUI

### 2.4 Memory Architecture

```
User Intent
    ↓
┌─────────────────────────────────────────────┐
│           MCP Bridge (Universal Glue)        │
└─────────────────────────────────────────────┘
    ↓                    ↓
 Honcho (Hot)       Open Brain (Deep)
 Session State       Semantic KB
 User Preferences    Long-term Facts
 Agent Traits        Project Research
```

### 2.5 3-Layer Internal Architecture (Per Agent)

| Layer | Function | Example |
|-------|----------|---------|
| **Directive** | SOPs written in Markdown | `directives/scrape_website.md` |
| **Orchestration** | Intelligent routing, decision-making | Reading directives, calling scripts in order |
| **Execution** | Deterministic Python scripts | `execution/scrape_single_site.py` |

**Why:** Prevents error compounding (90% × 5 steps = 59%). Pushing complexity into deterministic code = reliable, testable execution.

---

## 3. Feature Requirements

### 3.1 Core Features (Must Have)

| ID | Feature | Description | Priority |
|----|---------|-------------|----------|
| F1 | **Multica Dashboard** | Self-hosted Linear-style UI for task management | Must Have |
| F2 | **Claude Code Orchestrator** | Lead orchestrator; breaks down tasks, spawns sub-agents | Must Have |
| F3 | **Agent Registration** | Register Claude Code, Hermes, Agent Zero as team members | Must Have |
| F4 | **MCP Bridge Setup** | Configure MCP server for shared tools/memory | Must Have |
| F5 | **Shared Memory Server** | Deploy Honcho + Open Brain MCP servers | Must Have |
| F6 | **Docker Compose Stack** | Single `docker compose up -d` for full stack | Must Have |
| F7 | **OpenFang Integration** | Install OpenFang as 24/7 infrastructure OS | Must Have |
| F8 | **Claude Code ↔ Hermes Handoff** | Claude Code reads shared memory, delegates to Hermes | Must Have |
| F9 | **4-Provider LLM Config** | OpenRouter + Ollama + Claude + OpenWebUI with failover | Must Have |
| F10 | **Windows + WSL Dual Support** | Docker containers run on both Windows and Linux | Must Have |

### 3.2 Secondary Features (Should Have)

| ID | Feature | Description |
|----|---------|-------------|
| F11 | **Honcho Peer Card** | Build user profile via `/honcho:interview` |
| F12 | **Self-Annealing Loops** | Agents update directives when errors occur |
| F13 | **Cron Scheduling** | OpenFang runs morning reports 24/7 |
| F14 | **Cross-Channel Memory** | Honcho maintains context across channels |
| F15 | **Sovereign File Ownership** | All files stored locally, nothing external |
| F16 | **Provider Auto-Switch** | Tasks route to cheapest/fastest available provider |

### 3.3 Future Features (Nice to Have)

| ID | Feature | Description |
|----|---------|-------------|
| F17 | **KiloClaw Integration** | Connect existing betting pipeline as execution agent |
| F18 | **Firecrawl Web Scraping** | Add Firecrawl as research tool for all agents |
| F19 | **Voice Interface** | Hermes voice mode as natural-language front-end |
| F20 | **Telegram Bot Control** | Control and monitor AIOS via Telegram |

---

## 4. Technical Requirements

### 4.1 Infrastructure

- **Database:** PostgreSQL 17 with pgvector extension
- **Container:** Docker + Docker Compose
- **MCP Protocol:** All agents connect via MCP for shared tool/memory access
- **Runtime:** OpenFang as bare-metal always-on OS layer
- **Local LLM:** OpenWebUI on localhost:11434

### 4.2 Port Map

| Service | Port | Purpose |
|---------|------|---------|
| Multica Frontend | 3000 | Task management UI |
| Multica Backend | 8080 | API server |
| Open Brain MCP | 4000 | Deep memory / semantic KB |
| Honcho API | 8000 | Hot memory / session state |
| PostgreSQL | 5432 | Shared database (pgvector) |
| OpenWebUI | 11434 | Local LLM backend |

### 4.3 Environment Variables

```bash
# === LLM Providers ===
OPENROUTER_API_KEY=sk-or-...
OLLAMA_API_KEY=your_ollama_key
OLLAMA_BASE_URL=https://ollama.com/v1
ANTHROPIC_API_KEY=sk-ant-...
LOCAL_LLM_BASE_URL=http://localhost:11434/v1
LOCAL_LLM_API_KEY=

# === Fallback Chain ===
LLM_PROVIDER_FALLBACK_ORDER=openrouter,ollama,claude,local

# === Database ===
DB_USER=multica
DB_PASSWORD=...
DB_NAME=multica_os

# === Memory Servers ===
OPENAI_API_KEY=...  # Open Brain embeddings + Honcho

# === Backup (CSE encrypted before upload) ===
WASABI_ACCESS_KEY=...
WASABI_SECRET_KEY=...
BACKBLAZE_B2_APPLICATION_KEY_ID=...
BACKBLAZE_B2_APPLICATION_KEY=...

# === Optional ===
FIRECRAWL_API_KEY=...
KILOCODE_API_KEY=...
```

---

## 5. Privacy & Backup Architecture

### 5.1 Encryption Strategy

**Client-Side Encryption (CSE-KMS):** All data is encrypted with AES-256 before leaving the local machine. Encryption keys are stored locally or in a hardware key manager — cloud providers only ever see ciphertext.

```
Local Machine
  ├── Honcho data  ──[AES-256]──►  Wasabi S3 (ciphertext only)
  ├── Open Brain KB ──[AES-256]──►  Backblaze B2 (ciphertext only)
  └── Sovereign files ──[AES-256]──►  Both providers (dual redundancy)
```

### 5.2 Backup Providers

| Provider | Durability | Egress Fees | Best For |
|----------|-----------|-------------|----------|
| **Wasabi** | 11 9's | **Zero** | Primary S3-compatible target, $6/TB/mo flat |
| **Backblaze B2 + Cloudflare** | 11 9's | **Zero** via Cloudflare | Secondary, cross-provider diversity |

---

## 6. Implementation Phases

### Phase 1 — Foundation + Claude Code Orchestrator
1. Deploy Docker Compose stack (Multica + pgvector + Open Brain + Honcho)
2. Install Claude Code (`npm install -g @anthropic-ai/claude-code`)
3. Configure all 4 LLM providers with fallback chain
4. Register Claude Code as first agent in Multica
5. Verify MCP connectivity between Claude Code, Hermes, and memory servers
6. **Deliverable:** Task in Multica → Claude Code orchestrates → result in shared memory

### Phase 2 — Execution Agents + Memory Layer
1. Register Hermes as execution agent, connect to Honcho
2. Register Agent Zero in Docker, connect to Open Brain
3. Run `/honcho:interview` to build user Peer Card
4. Configure Claude Code to query Honcho + Open Brain before delegating tasks
5. **Deliverable:** Claude Code reads memory, delegates to Hermes, Agent Zero picks up where left off

### Phase 3 — Full Stack + 24/7 Infrastructure
1. Deploy OpenFang on Linux (bare-metal or VM)
2. Configure cron jobs (morning reports via OpenFang)
3. Test dual-platform: Windows Docker Desktop + WSL2 both run full stack
4. **Deliverable:** Autonomous multi-agent team running 24/7

### Phase 4 — Extensions
1. Integrate KiloClaw betting pipeline (F17)
2. Add Firecrawl for autonomous web research (F18)
3. Telegram control interface (F20)
4. Voice mode (F19)

---

## 7. Success Metrics

| Metric | Target |
|--------|--------|
| Claude Code → Hermes handoff success rate | > 90% |
| Memory retrieval accuracy | Agents retrieve relevant context without re-prompting |
| System uptime | OpenFang runs 24/7 with no manual intervention |
| Error self-correction | Agents fix and update directives without human help |
| Task completion rate | > 80% of Multica-assigned tasks completed autonomously |
| Provider failover | Auto-switch succeeds within 5 seconds on provider failure |

---

## 8. Open Questions

| # | Question | Status |
|---|----------|--------|
| Q1 | Which local LLM backend? | **OpenWebUI** |
| Q2 | Claude Code as native npm or inside Docker? | Open |
| Q3 | Who owns the directive files — user or agent? | Open |
| Q4 | Failure mode if Honcho + Open Brain go down? | Open |
| Q5 | How do you interact day-to-day? (CLI, Telegram, Multica UI?) | Open |
| Q6 | OpenFang on Windows — WSL2 or Linux VM? | Open |

---

## 9. References

- Repo: https://github.com/GustheTrader/sovereign-aios
- Claude Code: https://code.claude.com
- Multica: https://github.com/multica-ai/multica
- OpenFang: https://github.com/openfang/openfang
- Open Brain: https://github.com/postnikov/open-brain
- Honcho: https://github.com/plastic-labs/honcho
- OpenWebUI: https://github.com/open-webui/open-webui
- MCP Protocol: https://modelcontextprotocol.io

---

*This PRD is a living document.*
