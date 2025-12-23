# NGX GENESIS Multi-Agent MVP

Sistema multiagente de 6 agentes usando Google ADK para validar la infraestructura NGX GENESIS antes de producción.

## Estado del Proyecto

| Fase | Estado | Descripción |
|------|--------|-------------|
| Phase 1 | **Completada** | Core Setup - 6 agentes funcionando |
| Phase 2 | Pendiente | Tools Integration (Supabase, APIs) |
| Phase 3 | Pendiente | A2A Protocol (comunicación distribuida) |
| Phase 4 | Pendiente | Production Deployment (Agent Engine) |

## Arquitectura

```
                     ┌─────────────┐
                     │   GENESIS   │
                     │ Orchestrator│
                     │ gemini-2.5  │
                     │    -pro     │
                     └──────┬──────┘
                            │
       ┌────────────────────┼────────────────────┐
       │         │          │          │         │
 ┌─────▼────┐ ┌──▼───┐ ┌────▼───┐ ┌────▼───┐ ┌───▼───┐
 │  BLAZE   │ │ SAGE │ │ SPARK  │ │ STELLA │ │ LOGOS │
 │ Training │ │ Nutr │ │ Habits │ │ Mindset│ │  Edu  │
 │  flash   │ │flash │ │  flash │ │  flash │ │ flash │
 └──────────┘ └──────┘ └────────┘ └────────┘ └───────┘
```

## Quick Start

### 1. Clonar repositorio

```bash
git clone https://github.com/270aldo/genesis_tests_adk-.git
cd genesis_tests_adk-
```

### 2. Setup del entorno

```bash
# Crear entorno virtual
python -m venv venv
source venv/bin/activate  # Linux/Mac
# o: venv\Scripts\activate  # Windows

# Instalar dependencias
pip install -r requirements.txt
```

### 3. Configurar API Key

```bash
# Variable de entorno
export GOOGLE_API_KEY="tu-api-key-de-google-ai-studio"
```

### 4. Ejecutar

```bash
# Interfaz web interactiva (recomendado)
adk web .

# Después abre http://localhost:8000
# Selecciona "genesis_agent" en el selector de apps
```

## Agentes

| Agente | Dominio | Modelo | Personalidad |
|--------|---------|--------|--------------|
| **GENESIS** | Orquestación | gemini-2.5-pro | INTJ - Estratégico |
| **BLAZE** | Entrenamiento | gemini-2.5-flash | ESTP - Intenso |
| **SAGE** | Nutrición | gemini-2.5-flash | Educativo |
| **SPARK** | Hábitos | gemini-2.5-flash | Alentador |
| **STELLA** | Mindset | gemini-2.5-flash | INFJ - Empático |
| **LOGOS** | Educación | gemini-2.5-flash | Didáctico |

## Test Cases

Queries de prueba para validar el routing:

| Query | Agente Esperado | Estado |
|-------|-----------------|--------|
| "Quiero ganar músculo" | BLAZE | Verificado |
| "Qué como para bajar de peso" | SAGE | - |
| "No logro ser consistente" | SPARK | - |
| "Me saboteo mentalmente" | STELLA | Verificado |
| "Por qué necesito proteína" | LOGOS | - |
| "Plan completo de transformación" | GENESIS → Multiple | - |

## Estructura del Proyecto

```
ngx-genesis-mvp/
├── genesis_agent/
│   ├── __init__.py          # Export root_agent
│   └── agent.py             # GENESIS + 5 especialistas
├── .gitignore
├── requirements.txt         # google-adk>=1.21.0
├── CLAUDE.md                # Contexto para desarrollo
├── CHANGELOG.md             # Historial de cambios
└── README.md
```

## Desarrollo

### Comandos útiles

```bash
# Iniciar servidor de desarrollo
adk web .

# Ver logs detallados
adk web . --verbose

# Ejecutar en modo CLI
adk run genesis_agent
```

### Convenciones de commits

Seguimos [Conventional Commits](https://www.conventionalcommits.org/):

```
feat(agent): add new specialist agent
fix(routing): correct delegation logic
docs(readme): update setup instructions
refactor(genesis): simplify orchestration logic
```

## Roadmap Phase 2

- [ ] Integrar MCP Tools para Supabase
- [ ] Agregar Vertex AI RAG para knowledge base
- [ ] Implementar session state para contexto
- [ ] Modularizar agentes en archivos separados
- [ ] Agregar tests automatizados

## Referencias

- [Google ADK Documentation](https://google.github.io/adk-docs/)
- [A2A Protocol Spec](https://google.github.io/A2A/)

---

**NGX GENESIS** - *"Rinde hoy. Vive mejor mañana."*
