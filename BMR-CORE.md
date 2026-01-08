BMR-CORE.md

BMad Materials Research — Core Specification (v1.0)

⸻

1. Purpose and Scope

BMR (BMad Materials Research) is a cloud-first, agent-orchestrated framework for accelerating materials research and development through structured reasoning, experiment design, simulation, and machine learning, governed by cost- and confidence-based autonomy.

BMR is wedge-agnostic. Domain-specific behavior (magnets, batteries, capacitors, etc.) is implemented via adapters and must not modify core rules.

This document defines the constitutional rules of the system.

⸻

2. Core Principles (Immutable)
	1.	Reproducibility over novelty
Any result that cannot be replayed from stored manifests is invalid.
	2.	Agents reason; systems execute
LLM agents propose actions. Deterministic runners execute tools.
	3.	Autonomy is earned
Automation increases only with demonstrated predictive reliability.
	4.	Human approval is risk-based
Humans approve risk (cost × uncertainty), not routine execution.
	5.	Negative results are first-class artifacts
Failed hypotheses and null results must be preserved.
	6.	Every decision must be defensible
All outputs must cite assumptions, inputs, and uncertainty.

⸻

3. System Architecture (Logical)

User / API / CLI
      │
      ▼
BMR Orchestration Kernel
(stateful workflows, gates, DAGs)
      │
      ├── Agent Layer (multi-LLM, reasoning only)
      │
      ├── Tool Execution Layer (ML, simulation, DB, lab)
      │
      ▼
Provenance, Memory & Knowledge Layer
(graphs, artifacts, embeddings, audit logs)


⸻

4. Orchestration Kernel

The Orchestration Kernel is the authoritative system of record.

4.1 Responsibilities
	•	stateful workflow execution (pause / resume / retry)
	•	experiment DAG management
	•	approval gate enforcement
	•	cost estimation and reconciliation
	•	artifact versioning and lineage

4.2 Determinism Requirements
	•	all executions driven by immutable manifests
	•	tool versions recorded
	•	random seeds fixed when possible

⸻

5. Agent Contract

5.1 Allowed Agent Actions

Agents may:
	•	generate hypotheses
	•	design experiments (DOE)
	•	analyze results
	•	recommend next actions
	•	generate reports

Agents may not:
	•	execute tools directly
	•	mutate stored artifacts
	•	bypass approval gates

⸻

5.2 Mandatory Output Schema

Every agent output must include these base fields:

{
  "intent": "string",
  "assumptions": ["..."],
  "inputs_used": ["artifact_ids"],
  "confidence": 0.0,
  "uncertainty_notes": "string",
  "recommended_next_actions": []
}

Agent-specific schemas (defined in `agent_prompts.md`) extend this base with role-specific fields. Role-specific fields replace the generic `output` wrapper. Non-conforming outputs are rejected.

⸻

6. Canonical Agent Roles
	•	Research Lead (RL) — defines objectives and constraints
	•	Hypothesis Agent (HYPO) — generates competing hypotheses
	•	DOE Agent (DOE) — designs cost-aware experiments
	•	Simulation Orchestrator (SIM) — assigns fidelity and methods
	•	ML Agent (ML) — trains and validates surrogate models
	•	Runner (SYSTEM) — executes manifests deterministically
	•	Skeptic / QA (SKEP) — challenges conclusions and risk
	•	Scribe / IP (SCRIBE) — produces decision-ready artifacts

⸻

7. Workflow Model

Workflows are state machines, not conversations.

Each workflow defines:
	•	ordered steps
	•	required inputs and outputs
	•	approval gates
	•	failure handling

Workflows must be resumable from any step.

⸻

8. Tool Execution Layer

8.1 Tool Contract

All tools must declare:

{
  "tool_name": "string",
  "version": "semver",
  "input_schema": {},
  "output_schema": {},
  "estimated_cost_usd": 0.0,
  "estimated_runtime_sec": 0,
  "determinism": "deterministic | stochastic",
  "side_effects": "none"
}

Tools are invoked only by the Runner.

8.2 Fidelity Hierarchies

BMR uses two complementary fidelity systems:

**T-Tiers (Implementation Fidelity)** — Within a single tool class, T-tiers represent mesh resolution, convergence criteria, or computational intensity.

| Tier | Meaning | Example |
|------|---------|---------|
| T0 | Fast screening | FEMM 2D, coarse mesh |
| T1 | Standard fidelity | Elmer 3D coarse |
| T2 | Refined fidelity | Elmer 3D refined mesh |
| T3 | High fidelity | Coupled EM-thermal + CFD |

T-tiers are defined per implementation in the Tool Registry (`tool_registry_moat_spec_v0_2_0.json`).

**L-Levels (Method Fidelity)** — Across method classes, L-levels represent the physics fidelity ladder from heuristics to first-principles.

| Level | Method Class | Typical Cost |
|-------|--------------|--------------|
| L0 | Literature heuristics | Negligible |
| L1 | ML screening | Low |
| L2 | Physics-informed surrogates | Low-Medium |
| L3 | CALPHAD / phase models | Medium |
| L4 | Targeted DFT | High |
| L5 | Full micromagnetics | High |

L-levels are defined per wedge adapter (e.g., `BMR-MAGNETS-ADAPTER.md`).

**Escalation Policy:** Start at lowest T-tier within the required L-level. Escalate only when uncertainty reduction justifies cost.

⸻

9. Provenance & Knowledge

9.1 Artifact Types
	•	hypotheses
	•	DOE plans
	•	simulation manifests
	•	raw results
	•	processed analyses
	•	trained models
	•	decision memos

Artifacts are immutable and versioned.

9.2 Knowledge Graph

Entities:
	•	materials
	•	properties
	•	processes
	•	hypotheses

Relationships:
	•	supports
	•	contradicts
	•	derived_from

⸻

10. Cost Accounting

10.1 Cost Manifest (Required)

{
  "compute_cost": "usd",
  "simulation_cost": "usd",
  "ml_cost": "usd",
  "lab_cost": "usd | null",
  "total_estimated_cost": "usd"
}

Actual cost must be reconciled post-run.

⸻

11. Confidence Scoring

Each campaign × wedge × objective maintains a rolling Confidence Score (CS):

CS ∈ [0.0, 1.0]

CS controls autonomy gates. See Section 16 for detailed calculation methodology, weights, update rules, and threshold tables.

⸻

12. Approval Gates

A DOE batch is auto-approved iff:

(total_cost ≤ cost_threshold)
AND
(confidence_score ≥ confidence_threshold)
AND
(no Skeptic veto)

Otherwise, human approval is required.

⸻

13. Failure & Rollback

13.1 System-Level Failure Classes

	•	tool execution — tool failed to complete
	•	data corruption — artifact integrity compromised
	•	model instability — surrogate model diverged or extrapolated
	•	hypothesis collapse — all hypotheses refuted without replacement

13.2 Tool-Level Failure Classes (from Tool Registry)

Tool-level failures map to system-level classes:

| Tool Failure | System Class |
|--------------|--------------|
| non_convergence | tool execution |
| mesh_quality | tool execution |
| time_step_instability | tool execution |
| material_model_invalid | model instability |
| out_of_domain | model instability |
| license_or_env | tool execution |
| unknown | tool execution |

13.3 Failure Handling Rules

	•	partial results preserved
	•	confidence score penalized
	•	repeated failures escalate to human review

⸻

14. Security & Compliance (Assumed)
	•	tenant isolation
	•	encrypted artifacts
	•	per-run access controls
	•	full audit logs

⸻

15. What BMR Is Not
	•	not an autonomous scientist
	•	not a black-box discovery engine
	•	not a single-model system

BMR is a decision acceleration system.

⸻

16. Confidence Score Calculation

The Confidence Score (CS) is a rolling metric that controls autonomy escalation.

16.1 Inputs

| Input | Weight | Description |
|-------|--------|-------------|
| Prediction accuracy | 0.30 | Mean error of surrogate vs ground truth |
| Uncertainty calibration | 0.20 | Coverage of predicted intervals |
| Rank stability | 0.20 | Consistency of top-k rankings across iterations |
| Hypothesis hit rate | 0.15 | Fraction of hypotheses supported vs refuted |
| Reproducibility | 0.15 | Variance across repeated runs |

16.2 Update Rules

- CS increases when predictions align with outcomes
- CS decreases on unexpected failures or rank inversions
- CS is time-decayed (recent runs weighted more heavily)
- CS is model-version aware (resets partially on major model changes)

16.3 Thresholds

| CS Range | Autonomy Level |
|----------|----------------|
| 0.0 - 0.4 | Human approval required for all DOE |
| 0.4 - 0.7 | Auto-approve low-cost batches only |
| 0.7 - 0.9 | Auto-approve medium-cost batches |
| 0.9 - 1.0 | Full autonomy (high-cost requires Skeptic review) |

⸻

17. Versioning Strategy

17.1 Schema Versioning

All versioned artifacts use semantic versioning: `MAJOR.MINOR.PATCH`

- **MAJOR**: Breaking changes to schema structure or semantics
- **MINOR**: Backward-compatible additions (new optional fields, new tools)
- **PATCH**: Bug fixes, documentation clarifications

17.2 Version Locations

| Artifact | Version Field | Current |
|----------|---------------|---------|
| Tool Registry | `schema_version` | 0.2.0 |
| BMR-CORE | Document header | 1.0 |
| Agent Prompts | Document header | 1.0 |
| Wedge Adapters | Document header | 1.0 |

17.3 Compatibility Rules

- Consumers MUST reject schemas with unsupported MAJOR versions
- Consumers SHOULD accept schemas with higher MINOR versions (ignore unknown fields)
- Artifacts MUST include version in metadata for provenance

17.4 Deprecation Policy

- Deprecated fields are marked with `"deprecated": true` and `"deprecated_reason"`
- Deprecated fields are supported for at least 2 MINOR versions before removal
- Removal requires MAJOR version increment

⸻

18. v1 Exit Criteria

BMR Core v1 is complete when:
	•	one closed-loop campaign executes end-to-end
	•	all decisions are replayable
	•	cost and confidence gates function correctly

⸻

End of BMR-CORE.md
