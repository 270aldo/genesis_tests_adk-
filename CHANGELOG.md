# Changelog

All notable changes to NGX GENESIS MVP.

Format based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

## [Unreleased] - Phase 2 Planning

### Planned
- Modular agent structure (separate files per agent)
- MCP Tools integration for Supabase
- Vertex AI RAG for knowledge base
- Session state for conversation context
- Automated testing suite

---

## [0.1.0] - 2024-12-22

### Phase 1: Core Setup - COMPLETE

First working version of the NGX GENESIS multi-agent system.

### Added
- **GENESIS Orchestrator** (`gemini-2.5-pro`)
  - Central routing logic
  - Sub-agent delegation via `transfer_to_agent`
  - INTJ personality profile

- **BLAZE** - Training Specialist (`gemini-2.5-flash`)
  - Domain: Strength, hypertrophy, periodization
  - Personality: ESTP - Intense but not toxic

- **SAGE** - Nutrition Specialist (`gemini-2.5-flash`)
  - Domain: Nutritional science, macros, timing
  - Personality: Educational, non-judgmental

- **SPARK** - Habits Specialist (`gemini-2.5-flash`)
  - Domain: Habit formation, systems, consistency
  - Personality: Encouraging, celebrates small wins

- **STELLA** - Mindset Specialist (`gemini-2.5-flash`)
  - Domain: Mental performance, psychological barriers
  - Personality: INFJ - Empathetic, empowering

- **LOGOS** - Education Specialist (`gemini-2.5-flash`)
  - Domain: Explanations, the "why" behind advice
  - Personality: Didactic, patient

### Technical Details
- Framework: Google ADK 1.21.0
- Models: gemini-2.5-pro (orchestrator), gemini-2.5-flash (specialists)
- Architecture: Single-file pattern in `genesis_agent/agent.py`
- Run command: `adk web .` from project root

### Verified
- [x] Routing to BLAZE ("Quiero ganar músculo")
- [x] Routing to STELLA ("Me saboteo mentalmente")
- [x] ADK web interface working at localhost:8000
- [x] Sub-agent delegation via `transfer_to_agent` tool

### Known Limitations
- No tools integration (intentional for Phase 1)
- No persistent state between sessions
- All agents in single file (to be modularized in Phase 2)
- gemini-3 models not available, using gemini-2.5

---

## Roadmap

### Phase 2: Tools Integration
- [ ] Supabase integration for user data
- [ ] Vertex AI RAG for knowledge base
- [ ] Analytics tools for progress tracking
- [ ] Modular file structure

### Phase 3: A2A Protocol
- [ ] Distributed agent deployment
- [ ] Agent Cards for discovery
- [ ] Cross-service communication

### Phase 4: Production
- [ ] Vertex AI Agent Engine deployment
- [ ] Monitoring and observability
- [ ] Rate limiting and auth
- [ ] Full 13-agent system
