# CLAUDE.md - NGX GENESIS MVP

## Project Overview

NGX GENESIS is a multi-agent fitness coaching system built on Google ADK 1.21.0. The architecture consists of a central orchestrator (GENESIS) coordinating 5 specialist agents (BLAZE, SAGE, SPARK, STELLA, LOGOS) to provide comprehensive fitness guidance in Spanish.

**Current Phase**: Phase 1 Complete - Core Setup
**Goal**: Validate ADK multi-agent infrastructure before production deployment
**Target Audience**: Spanish-speaking fitness users

## Tech Stack

| Component | Technology | Notes |
|-----------|------------|-------|
| Framework | Google ADK 1.21.0 | Multi-agent orchestration |
| Language | Python 3.11+ | Required for ADK |
| Orchestrator Model | `gemini-2.5-pro` | GENESIS only |
| Specialist Models | `gemini-2.5-flash` | All 5 specialists |
| Dev Server | `adk web .` | localhost:8000 |

## Project Structure

```
genesis_tests_adk-/
├── genesis_agent/
│   ├── __init__.py       # Exports root_agent
│   └── agent.py          # All 6 agents (200 lines)
├── .gitignore            # Python, venv, ADK, IDE exclusions
├── requirements.txt      # google-adk>=1.21.0
├── CLAUDE.md             # This file (AI assistant context)
├── CHANGELOG.md          # Version history
└── README.md             # User-facing documentation
```

## Agent Architecture

```
                     ┌─────────────────┐
                     │     GENESIS     │
                     │   Orchestrator  │
                     │  gemini-2.5-pro │
                     │   INTJ Profile  │
                     └────────┬────────┘
                              │
       ┌──────────┬───────────┼───────────┬──────────┐
       │          │           │           │          │
 ┌─────▼────┐ ┌───▼───┐ ┌─────▼────┐ ┌────▼────┐ ┌───▼────┐
 │  BLAZE   │ │ SAGE  │ │  SPARK   │ │ STELLA  │ │ LOGOS  │
 │ Training │ │ Nutr. │ │  Habits  │ │ Mindset │ │  Edu   │
 │   ESTP   │ │ Educ. │ │ Encour.  │ │  INFJ   │ │ Didact │
 └──────────┘ └───────┘ └──────────┘ └─────────┘ └────────┘
```

### Agent Routing Matrix

| Query Pattern | Target Agent | Routing Keyword |
|---------------|--------------|-----------------|
| Muscle, strength, exercise | BLAZE | entrenamiento, fuerza, hipertrofia |
| Food, diet, macros | SAGE | nutrición, comer, calorías |
| Consistency, discipline | SPARK | hábitos, consistencia, sistema |
| Mental blocks, sabotage | STELLA | mindset, saboteo, mental |
| Explanations, science | LOGOS | por qué, explicar, entender |

## Commands

```bash
# Setup (one-time)
python -m venv venv
source venv/bin/activate        # Linux/Mac
pip install -r requirements.txt
export GOOGLE_API_KEY="your-key"

# Development (run from project root, NOT inside genesis_agent/)
adk web .                       # Web UI at localhost:8000
adk web . --verbose             # With debug logs

# CLI mode
adk run genesis_agent
adk run genesis_agent --prompt "Quiero ganar músculo"
```

## Key Patterns

### Agent Definition Pattern

```python
from google.adk.agents import Agent

agent_name = Agent(
    name="agent_name",                    # Lowercase, no spaces
    model="gemini-2.5-flash",             # Or gemini-2.5-pro for orchestrator
    description="One-line routing hint",  # CRITICAL: Used by orchestrator for delegation
    instruction="""Multi-line system prompt

    PERSONALIDAD:
    - Tone and style directives

    DOMINIO:
    - Subject matter expertise

    SIEMPRE:
    - Required behaviors

    NUNCA:
    - Forbidden behaviors
    """,
)
```

### Root Agent with Sub-agents

```python
root_agent = Agent(
    name="genesis",
    model="gemini-2.5-pro",
    description="Orquestador central",
    instruction="""...""",
    sub_agents=[blaze, sage, spark, stella, logos],  # List of specialist agents
)
```

### Module Export Pattern

```python
# genesis_agent/__init__.py
from .agent import root_agent  # ADK discovers this automatically
```

## Development Guidelines

### DO's

- Use the `description` field for routing hints (CRITICAL for orchestrator delegation)
- Keep instructions focused on one domain per agent
- Export `root_agent` from `genesis_agent/__init__.py`
- Use Spanish in agent instructions (target audience)
- Test routing with varied queries in web UI
- Run `adk web .` from project root directory
- Define clear domain boundaries between agents

### DON'Ts

- Add tools in Phase 1 (keep it simple for infrastructure validation)
- Create complex state management yet
- Modify agent personalities without consulting PRD/Voice Bible
- Run `adk web ./genesis_agent` (wrong path - use `.` from root)
- Let agents answer outside their domain (e.g., BLAZE giving nutrition advice)
- Use models other than specified (gemini-2.5-pro/flash)

## Validation Checklist

Phase 1 routing tests (run in `adk web .` interface):

- [x] "Quiero ganar músculo" → BLAZE
- [x] "Me saboteo mentalmente" → STELLA
- [ ] "Qué debo comer" → SAGE
- [ ] "No puedo ser consistente" → SPARK
- [ ] "Por qué es importante la proteína" → LOGOS
- [ ] Multi-domain query → GENESIS coordinates multiple agents

## Troubleshooting

| Issue | Solution |
|-------|----------|
| `ModuleNotFoundError: google.adk` | Activate venv, run `pip install -r requirements.txt` |
| `No app found` in web UI | Run `adk web .` from project root, not inside `genesis_agent/` |
| Agent not responding | Check `GOOGLE_API_KEY` is set correctly |
| Wrong agent selected | Improve `description` field with clearer routing keywords |
| Infinite delegation loop | Ensure clear domain boundaries in instructions |

## Phase 2 Planning

### Target Architecture

```
genesis_agent/
├── __init__.py           # Export root_agent
├── agent.py              # GENESIS orchestrator + imports
├── agents/
│   ├── __init__.py
│   ├── blaze.py          # Agent + tools
│   ├── sage.py
│   ├── spark.py
│   ├── stella.py
│   └── logos.py
└── tools/
    ├── __init__.py
    ├── supabase_tools.py
    ├── rag_tools.py
    └── analytics_tools.py
```

### Tools Integration Pattern

```python
from google.adk.tools import FunctionTool

@FunctionTool
def get_workout_plan(user_id: str, goal: str) -> dict:
    """Retrieve workout plan from Supabase."""
    # Implementation
    return plan

blaze = Agent(
    name="blaze",
    model="gemini-2.5-flash",
    tools=[get_workout_plan],
    # ...
)
```

### Planned Dependencies

```
google-adk>=1.21.0
supabase>=2.0.0
google-cloud-aiplatform>=1.40.0  # Vertex AI RAG
```

## Project Roadmap

| Phase | Status | Description |
|-------|--------|-------------|
| Phase 1 | **Complete** | Core Setup - 6 agents with routing |
| Phase 2 | Pending | Tools Integration (Supabase, RAG) |
| Phase 3 | Pending | A2A Protocol (distributed agents) |
| Phase 4 | Pending | Production (Vertex AI Agent Engine) |

## Commit Conventions

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
feat(agent): add new specialist agent
fix(routing): correct delegation logic
docs(readme): update setup instructions
refactor(genesis): simplify orchestration logic
```

## Notes

- This is an MVP for infrastructure testing, not production
- Agent personalities are defined in NGX Agent Voice Bible
- Full production system will have 13 agents (currently 6)
- A2A protocol integration planned for inter-service communication
- All agent instructions are in Spanish for target audience
