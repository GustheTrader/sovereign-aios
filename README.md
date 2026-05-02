# Sovereign AIOS

> **Self-hosted, privacy-first agentic operating system.**

Private repository. See `docs/EXECUTIVE_SUMMARY.md` for an overview.

## Quick Start

```bash
# 1. Clone
git clone https://github.com/GustheTrader/sovereign-aios.git
cd sovereign-aios

# 2. Configure environment
cp .env.example .env
# Edit .env with your API keys

# 3. Deploy stack
docker compose up -d

# 4. Verify
docker compose ps
```

## Services

| Service | Port | Description |
|---------|------|-------------|
| Multica | http://localhost:3000 | Task management UI |
| Open Brain | http://localhost:4000 | Deep memory (semantic KB) |
| Honcho | http://localhost:8000 | Hot memory (session state) |
| OpenWebUI | http://localhost:11434 | Local LLM backend |
| PostgreSQL | localhost:5432 | Shared database (pgvector) |

## Documentation

- `docs/EXECUTIVE_SUMMARY.md` — Partner-facing overview
- `PRD.md` — Full product requirements document

## Architecture

```
Claude Code (Orchestrator)
    ↓ MCP
Hermes + Agent Zero (Execution)
    ↓ MCP
Honcho (Hot Memory) ←→ Open Brain (Deep Memory)
    ↓         ↓
OpenWebUI (Local LLM)    PostgreSQL + pgvector
```

## License

Private. All rights reserved.
