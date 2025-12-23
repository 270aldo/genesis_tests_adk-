# CLAUDE.md - NGX GENESIS MVP

## Project Overview

Multi-agent system using Google ADK 1.21.0. Architecture: GENESIS (orchestrator) + 5 specialists (BLAZE, SAGE, SPARK, STELLA, LOGOS).

**Current Phase**: Phase 1 Complete - Core Setup
**Goal**: Validate ADK multi-agent infrastructure before production deployment.

## Tech Stack

- **Framework**: Google ADK 1.21.0
- **Language**: Python 3.11+
- **Models**:
  - `gemini-2.5-pro` (GENESIS orchestrator)
  - `gemini-2.5-flash` (specialist agents)
- **Testing**: `adk web .` for interactive UI

## Project Structure

```
ngx-genesis-mvp/
├── genesis_agent/
│   ├── __init__.py       # Export root_agent
│   └── agent.py          # All 6 agents (single file pattern)
├── .gitignore
├── requirements.txt
├── CLAUDE.md             # This file
├── CHANGELOG.md
└── README.md
```

## Key Patterns

### ADK Agent Definition

```python
from google.adk.agents import Agent

agent_name = Agent(
    name="agent_name",
    model="gemini-2.5-flash",
    description="One-line for routing",  # CRITICAL for sub-agent selection
    instruction="""Multi-line system prompt...""",
)
```

### Root Agent with Sub-agents

```python
root_agent = Agent(
    name="genesis",
    model="gemini-2.5-pro",
    sub_agents=[blaze, sage, spark, stella, logos],
    instruction="""...""",
)
```

### Export Pattern

```python
# genesis_agent/__init__.py
from .agent import root_agent
```

## Commands

```bash
# Development (from project root)
adk web .                    # Interactive UI at localhost:8000

# CLI mode
adk run genesis_agent        # Run with CLI

# With specific prompt
adk run genesis_agent --prompt "Quiero ganar músculo"
```

## DO's

- Use `description` field for routing hints (CRITICAL)
- Keep instructions focused on one domain per agent
- Export `root_agent` from `genesis_agent/__init__.py`
- Use Spanish in agent instructions (target audience)
- Test routing with varied queries
- Run `adk web .` from project root (not inside genesis_agent/)

## DON'Ts

- Add tools in Phase 1 (keep it simple)
- Create complex state management yet
- Modify agent personalities without consulting PRD
- Run `adk web ./genesis_agent` (wrong - use `.` from parent)

## Validation Checklist

Phase 1 routing tests:

- [x] "Quiero ganar músculo" → BLAZE
- [x] "Me saboteo mentalmente" → STELLA
- [ ] "Qué debo comer" → SAGE
- [ ] "No puedo ser consistente" → SPARK
- [ ] "Por qué es importante la proteína" → LOGOS

## Phase 2 Planning

### Architecture Evolution

Current (Phase 1):
```
genesis_agent/
└── agent.py          # All agents in single file
```

Target (Phase 2):
```
genesis_agent/
├── agent.py              # GENESIS + imports
├── agents/
│   ├── blaze.py          # Agent + tools
│   ├── sage.py
│   ├── spark.py
│   ├── stella.py
│   └── logos.py
└── tools/
    ├── supabase_tools.py
    ├── rag_tools.py
    └── analytics_tools.py
```

### Tools Integration

```python
from google.adk.tools import FunctionTool

@FunctionTool
def get_workout_plan(user_id: str, goal: str) -> dict:
    """Retrieve workout plan from Supabase."""
    # ... implementation
    return plan

blaze = Agent(
    name="blaze",
    tools=[get_workout_plan],
    # ...
)
```

### Dependencies to Add (Phase 2)

```
google-adk>=1.21.0
supabase>=2.0.0
google-cloud-aiplatform>=1.40.0  # For Vertex AI RAG
```

## Notes

- This is MVP for infrastructure testing, not production
- Agent personalities from NGX Agent Voice Bible
- Full system will have 13 agents (currently 6)
- A2A protocol integration planned for Phase 3
