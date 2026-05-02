# Sovereign AIOS — Executive Summary

> **Private & Confidential — For Partners Only**

---

## What We Are Building

**Sovereign AIOS** is a self-hosted, privacy-first agentic operating system that coordinates a team of specialized AI agents — each doing what it does best — sharing context through a common brain. Nothing leaves the local infrastructure. No human approval gates. Runs 24/7.

**Core thesis:** Single AI agents compound errors downward (90% accuracy × 5 steps = 59% success). A team of purpose-built agents, sharing memory, outperforms any single model — and stays fully under your control.

---

## The Problem

| Issue | Implication |
|-------|-------------|
| Single AI agents compound errors across multi-step tasks | Unreliable for complex autonomous workflows |
| Cloud AI = your data leaves your hands | No sovereignty, no privacy, regulatory risk |
| Provider dependency (OpenAI, etc.) | Single point of failure, latency, cost variance |
| No memory persistence between sessions | Every conversation starts from scratch |
| Manual handoff between tools/agents | Can't run autonomously 24/7 |

---

## The Solution

A sovereign agentic OS built on four pillars:

### 1. Multi-Agent Orchestra
A team of specialized agents, each assigned to its strongest use case:

| Agent | Role | Best For |
|-------|------|----------|
| **Claude Code** | Lead Orchestrator | Complex planning, breaking down tasks, spawning sub-agents |
| **Hermes** | Execution Expert | Fast code loops, data analysis, research, CLI gateway |
| **Agent Zero** | Autonomous Developer | Building its own tools, high-complexity coding |
| **OpenFang** | Infrastructure OS | 24/7 background workers, bare-metal performance |

### 2. 4-Leg LLM Strategy (Provider Resilience)
No single point of failure. Automatic failover from primary to backup:

```
Task Request
    ↓
OpenRouter (100+ models, single API key, auto-failover)
    ↓ (fallback)
Ollama Cloud (fast, cheap tasks)
    ↓ (fallback)
Claude OAuth (complex reasoning — Anthropic direct)
    ↓ (fallback)
OpenWebUI (local LLM — fully sovereign, zero cloud dependency)
```

### 3. Memory Architecture
Two-tier memory system so agents share context instantly:

- **Honcho (Hot Memory):** Real-time session state, user preferences, agent social traits
- **Open Brain (Deep Memory):** Semantic KB with pgvector — long-term facts, project research, cross-agent knowledge sharing

### 4. Sovereign Data Architecture
**Full privacy by design:**

- All configuration, directives, memory, and brain files owned locally
- Nothing syncs externally without client-side AES-256 encryption
- Backup targets (Wasabi + Backblaze B2) receive only ciphertext
- User holds the encryption keys — even AWS can't read the data

---

## Edge Hunter Integration

The Sovereign AIOS connects directly to your existing infrastructure:

- **KiloClaw VPS** — existing betting pipeline becomes a specialized execution agent
- **Horse Racing Pipeline** — TRD PDFs + Equibase XML parsed and enriched via the shared memory stack
- **6 Crypto HF Models** — real-time market signals feed into the shared brain for cross-market analysis

---

## Technology Stack

| Layer | Technology |
|-------|-----------|
| Lead Orchestrator | Claude Code (native tmux + MCP) |
| Execution Agents | Hermes Agent + Agent Zero |
| Memory (Hot) | Honcho (MCP server) |
| Memory (Deep) | Open Brain (pgvector semantic KB) |
| Task Management | Multica (Linear-style UI, self-hosted) |
| Infrastructure OS | OpenFang (Rust-based, 24/7 bare-metal) |
| Protocol Layer | MCP Bridge (universal tool/memory sharing) |
| Container | Docker + Docker Compose |
| Database | PostgreSQL 17 + pgvector |
| Local LLM | OpenWebUI |
| Cloud Backup | Wasabi (primary) + Backblaze B2 (secondary) |

---

## Implementation Phases

| Phase | Focus | Deliverable |
|-------|-------|-------------|
| **Phase 1** | Foundation + Orchestrator | Docker Compose stack deployed; Claude Code registered in Multica; all 4 LLM providers configured with fallback chain |
| **Phase 2** | Execution Agents + Memory | Hermes + Agent Zero connected to Honcho + Open Brain; user preference profile built |
| **Phase 3** | 24/7 Infrastructure | OpenFang deployed; cron jobs for morning reports; Windows + WSL dual-platform verified |
| **Phase 4** | Extensions | KiloClaw integration, Firecrawl research tool, Telegram control interface |

---

## Competitive Advantages

| Advantage | Why It Matters |
|-----------|----------------|
| **Full Data Sovereignty** | Nothing leaves your infrastructure without your encryption keys |
| **Provider Resilience** | 4-leg LLM strategy — no single point of failure or vendor lock-in |
| **Memory Persistence** | Agents remember across sessions, learn from each other's work |
| **Edge Hunter Native** | Built from day one to integrate with your existing KiloClaw + crypto HF models |
| **Self-Healing** | Agents update their own directives when errors occur — system gets smarter over time |
| **Zero Human Approval Gates** | Runs autonomously; human in the loop only for high-stakes decisions |

---

## Risk Mitigation

| Risk | Mitigation |
|------|------------|
| Cloud provider outage | Auto-failover to backup LLM providers; local OpenWebUI as last resort |
| Memory server failure | OpenFang monitors and auto-restarts; dual backup to S3 |
| Agent directive drift | Centralized directive storage in shared brain |
| Hardware requirements | Phased deployment — cloud providers first, local LLM as supplement |
| Complex setup | Docker Compose single-command deploy; proven component stack |

---

## Status

**Current:** Phase 1 planning complete. PRD locked. Ready for foundation deployment.

**Next Steps:** Deploy Docker Compose stack (Multica + pgvector + Open Brain + Honcho), install Claude Code, configure 4-provider LLM strategy.

---

## References

- Repo: https://github.com/GustheTrader/sovereign-aios (private)
- PRD: `PRD.md` in this repository
- Claude Code: https://code.claude.com
- Multica: https://github.com/multica-ai/multica
- OpenFang: https://github.com/openfang/openfang
- Open Brain: https://github.com/postnikov/open-brain
- Honcho: https://github.com/plastic-labs/honcho
- OpenWebUI: https://github.com/open-webui/open-webui
