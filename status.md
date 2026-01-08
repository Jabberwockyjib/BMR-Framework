# Project Status

**Last Updated:** 2026-01-08

## Current State

BMR Framework is a complete, production-ready specification repository defining an AI-driven research orchestration system for magnet and electromagnetic device R&D. All core documents are fully developed, harmonized, and validated.

### Core Documents

| Document | Lines | Purpose |
|----------|-------|---------|
| `BMR-CORE.md` | ~380 | Core specification with fidelity hierarchies, confidence scoring, versioning |
| `BMR-WORKFLOWS.md` | ~270 | 4 canonical workflows with SAL integration |
| `BMR-TOOL-CONTRACTS.md` | ~240 | Tool execution interface specification |
| `agent_prompts.md` | ~315 | System prompts + JSON schemas for 7 agent roles |
| `SAL.md` | ~180 | Scientific Action Language with workflow mapping |
| `tool_registry_moat_spec_v0_2_0.json` | 753 | Physics graph, KPIs, implementations |

### Domain Adapters

| Adapter | Purpose |
|---------|---------|
| `BMR-MAGNETS-ADAPTER.md` | Permanent magnet wedge (Nd-Fe-B, RE-lean) |
| `BMR-ADAPTER-TEMPLATE.md` | Template for creating new domain adapters |
| `BMR-REFERENCE-CAMPAIGN.md` | Worked example: RE-reduction campaign |

### Validation & Tooling

| File | Purpose |
|------|---------|
| `schemas/tool_registry.schema.json` | JSON Schema for tool registry validation |
| `schemas/agent_output.schema.json` | JSON Schema for agent output validation |

## Recent Changes

### Gap Fixes (This Session)
- Created `README.md` with ASCII + mermaid architecture diagrams
- Added JSON validation schemas for tool registry and agent outputs
- Created `BMR-ADAPTER-TEMPLATE.md` for new domain adapters
- Added versioning strategy (Section 17) to BMR-CORE.md
- Added confidence score calculation details (Section 16) to BMR-CORE.md
- Cross-referenced SAL.md ↔ BMR-WORKFLOWS.md (bidirectional)
- Linked magnet_taxonomy_v1.md from BMR-MAGNETS-ADAPTER.md
- Archived old tool_registry_spec_v0_1_0.json to `/archive/`

### Harmonization (Previous Session)
- Added dual fidelity hierarchy (L-levels for methods, T-tiers for implementations)
- Standardized agent role abbreviations (RL, HYPO, DOE, SIM, ML, SKEP, SCRIBE)
- Added `inputs_used` field to all 7 agent output schemas
- Added failure classification mapping between tool-level and system-level failures
- Replaced undefined "Research Scout" agent with RL sub-task

## Known Issues

None critical. Repository is ready for use.

## Architecture Notes

```
BMR Framework
├── README.md                          # GitHub landing page
├── BMR-CORE.md                        # Constitutional rules
├── BMR-WORKFLOWS.md                   # State machine definitions
├── BMR-TOOL-CONTRACTS.md              # Tool interface spec
├── agent_prompts.md                   # LLM agent prompts
├── SAL.md                             # Scientific Action Language
├── tool_registry_moat_spec_v0_2_0.json # Physics graph + tools
├── magnet_taxonomy_v1.md              # Problem taxonomy
├── BMR-MAGNETS-ADAPTER.md             # Magnets wedge
├── BMR-ADAPTER-TEMPLATE.md            # New adapter template
├── BMR-REFERENCE-CAMPAIGN.md          # Worked example
├── CLAUDE.md                          # Claude Code guidance
├── schemas/
│   ├── tool_registry.schema.json      # Tool registry validation
│   └── agent_output.schema.json       # Agent output validation
└── archive/
    └── tool_registry_spec_v0_1_0.json # Deprecated v0.1.0
```

The framework is designed for LLM agents to consume and orchestrate physics simulations via tools like Elmer, FEMM, MuMax3, OpenFOAM, and AiiDA.
