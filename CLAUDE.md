# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

BMR (BMad Materials Research) Framework is an AI-driven research orchestration system for magnet and electromagnetic device R&D. This is a **specification repository**, not executable code—it defines schemas, agent prompts, and methodologies consumed by LLM agents and workflow orchestrators.

## Architecture

```
BMR Framework
├── Ontology Layer (tool_registry_moat_spec_v0_2_0.json/yaml)
│   ├── KPI definitions (field, motor, thermal, demag, micromagnetics, structural, materials)
│   ├── Physics graph: problem nodes → fidelity ladder → tool implementations
│   └── Failure classification (non_convergence, mesh_quality, time_step_instability, etc.)
├── Agent Layer (agent_prompts.md)
│   └── 7 agents: Research Lead, Hypothesis, DOE, Simulation Orchestrator, ML, Skeptic/QA, Scribe/IP
├── Scientific Method Layer (SAL.md)
│   └── CLAIM → INTERVENTION → TEST → UPDATE loop
├── Problem Taxonomy (magnet_taxonomy_v1.md)
│   └── 9 categories: atomic materials, microstructure, manufacturing, assemblies, motors, thermal, structural, uncertainty, inverse design
└── Tool Ecosystem (9 simulation tools)
    └── Elmer, FEMM, MuMax3, OOMMF, OpenFOAM, Gmsh, ParaView, Quantum ESPRESSO, AiiDA
```

## Key Files

| File | Purpose |
|------|---------|
| `tool_registry_moat_spec_v0_2_0.json` | Primary tool registry with physics graph, KPIs, implementations |
| `SAL.md` | Scientific Action Language - formal closed-loop research methodology |
| `magnet_taxonomy_v1.md` | Problem taxonomy mapping physics domains to tools |
| `agent_prompts.md` | System prompts and JSON schemas for 7 canonical agent roles |
| `BMR-CORE.md` | Core specification: principles, architecture, agent contracts |
| `BMR-WORKFLOWS.md` | Canonical workflows: hypothesis-sprint, doe-plan, closed-loop-iteration |
| `BMR-TOOL-CONTRACTS.md` | Tool execution interface specification |
| `BMR-MAGNETS-ADAPTER.md` | Permanent magnet wedge adapter |
| `BMR-REFERENCE-CAMPAIGN.md` | Worked reference campaign example |

## Core Design Principles

1. **Physics-First**: Nodes represent physical questions; tools are implementations
2. **Multi-Fidelity**: Two fidelity hierarchies — L-levels (method: L0-L5) and T-tiers (implementation: T0-T3). Start at lowest fidelity meeting uncertainty targets.
3. **Closed-Loop**: Every simulation produces standardized KPIs + uncertainty + provenance
4. **Failure-Aware**: Failed runs are classified and update constraints/priors
5. **Agent-Orchestrated**: Agents propose actions via structured JSON; no direct tool execution

## Tool Registry Structure (v0.2.0)

The physics graph maps problems to implementations:
- **Problem nodes** (e.g., P4_field_homogeneity, P6_demag_derating, P2_coercivity_mechanisms)
- **Fidelity ladders** escalate from fast screening (FEMM 2D) to high-fidelity (Elmer 3D + CFD)
- **Implementation registry** specifies resource requirements (CPUs, GPU, memory, wall time)

Artifact contracts require:
```
inputs/    outputs/    logs/
run.json   results.json   provenance.json
```

## Agent JSON Output Schemas

All agents must output structured JSON. Key required fields per agent type are defined in `agent_prompts.md`. Global guardrails prohibit untestable claims, require explicit uncertainty, and mandate provenance tracking.

## Working with This Repository

- When editing tool registry, maintain schema version and validate JSON/YAML consistency
- Agent prompts must include required JSON output schema fields
- SAL objects (CLAIM, INTERVENTION, TEST, UPDATE) have strict semantic requirements
- Taxonomy entries should specify physics domains and required KPIs
