BMR-WORKFLOWS.md

Canonical Executable Workflows (v1.0)

⸻

1. Purpose

This document defines the canonical workflows for the BMR framework. These workflows are state machines, not conversations.

They are designed to:
	•	be resumable
	•	be auditable
	•	enforce governance and approval gates
	•	produce replayable artifacts

All wedges (magnets, batteries, capacitors, etc.) must conform to these workflows.

⸻

2. Workflow Definition Model

Each workflow is defined as a deterministic sequence of steps.

workflow:
  name: string
  inputs: []
  steps:
    - id: string
      agent: role | SYSTEM
      action: description
      required_outputs: schema
      approval_gate: none | conditional | required
      failure_behavior: retry | abort | escalate

Workflows must be pauseable and resumable at any step.

2.1 Agent Role Abbreviations

| Abbrev | Role | Purpose |
|--------|------|---------|
| RL | Research Lead | Defines objectives and constraints |
| HYPO | Hypothesis Agent | Generates competing hypotheses |
| DOE | DOE Agent | Designs cost-aware experiments |
| SIM | Simulation Orchestrator | Assigns fidelity and methods |
| ML | ML Agent | Trains and validates surrogates |
| SYSTEM | Runner | Executes manifests deterministically |
| SKEP | Skeptic / QA | Challenges conclusions and risk |
| SCRIBE | Scribe / IP | Produces decision-ready artifacts |

⸻

3. Workflow: hypothesis-sprint

Purpose

Generate competing, falsifiable hypotheses grounded in physics, prior data, and constraints.

Inputs
	•	campaign objective
	•	constraints
	•	wedge adapter
	•	prior artifacts (optional)

⸻

Steps

Step HS-1 — Objective Confirmation
	•	Agent: RL
	•	Action: confirm objective and constraints
	•	Output: scoped objective artifact
	•	Gate: none
	•	Failure: abort

Step HS-2 — Prior Knowledge Scan
	•	Agent: RL (prior scan sub-task)
	•	Action: extract known mechanisms, priors, and failure modes
	•	Output: prior knowledge artifact
	•	Gate: none
	•	Failure: continue with warning

Step HS-3 — Hypothesis Generation
	•	Agent: HYPO
	•	Action: generate ≥3 competing hypotheses across ≥2 mechanism classes
	•	Output: hypothesis set artifact
	•	Gate: none
	•	Failure: retry

Step HS-4 — Skeptic Review
	•	Agent: SKEP
	•	Action: challenge completeness and assumptions
	•	Output: skeptic review artifact
	•	Gate: conditional (flagged → human)
	•	Failure: escalate

⸻

Required Outputs
	•	hypothesis set
	•	falsification criteria
	•	discriminator properties
	•	uncertainty notes

⸻

4. Workflow: doe-plan

Purpose

Design cost-aware, uncertainty-reducing experiment batches.

Inputs
	•	hypothesis set
	•	wedge adapter
	•	current confidence score
	•	budget constraints

⸻

Steps

Step DOE-1 — DOE Draft
	•	Agent: DOE
	•	Action: design experiment matrix
	•	Output: DOE draft artifact
	•	Gate: none
	•	Failure: retry

Step DOE-2 — Fidelity Assignment
	•	Agent: SIM
	•	Action: assign method and fidelity per experiment
	•	Output: simulation assignment artifact
	•	Gate: none
	•	Failure: retry

Step DOE-3 — Cost & Confidence Evaluation
	•	Agent: SYSTEM
	•	Action: evaluate cost vs thresholds and confidence score
	•	Output: approval decision artifact
	•	Gate: conditional
	•	Failure: abort

Step DOE-4 — Skeptic Veto
	•	Agent: SKEP
	•	Action: assess risk and force review if necessary
	•	Output: skeptic veto artifact
	•	Gate: conditional → required
	•	Failure: escalate

⸻

Required Outputs
	•	approved DOE batch
	•	cost manifest
	•	expected information gain

⸻

5. Workflow: closed-loop-iteration

Purpose

Execute experiments, update models, evaluate hypotheses, and decide next actions.

**Note:** This workflow implements the SAL (Scientific Action Language) loop: CLAIM → INTERVENTION → TEST → UPDATE. See [SAL.md](SAL.md) for formal semantics and object schemas.

Inputs
	•	approved DOE batch
	•	tool manifests
	•	prior models

⸻

Steps

Step CL-1 — Execution
	•	Agent: SYSTEM
	•	Action: execute tool manifests
	•	Output: raw execution artifacts
	•	Gate: none
	•	Failure: retry or partial continue

Step CL-2 — Model Update
	•	Agent: ML
	•	Action: update surrogate models and uncertainty
	•	Output: model update artifact
	•	Gate: none
	•	Failure: retry

Step CL-3 — Hypothesis Evaluation
	•	Agent: HYPO
	•	Action: evaluate hypotheses against results
	•	Output: hypothesis evaluation artifact
	•	Gate: none
	•	Failure: continue

Step CL-4 — Skeptic Audit
	•	Agent: SKEP
	•	Action: assess validity and overconfidence
	•	Output: skeptic audit artifact
	•	Gate: conditional
	•	Failure: escalate

Step CL-5 — Decision Memo
	•	Agent: SCRIBE
	•	Action: generate decision-ready memo
	•	Output: decision memo artifact
	•	Gate: required if next DOE exceeds autonomy
	•	Failure: retry

⸻

Required Outputs
	•	updated confidence score
	•	ranked candidates
	•	decision memo
	•	recommended next iteration

⸻

6. Workflow: decision-memo (Standalone)

Purpose

Generate a human-readable, decision-ready summary at any point.

Inputs
	•	selected artifacts

Steps

Step DM-1 — Synthesis
	•	Agent: SCRIBE
	•	Action: summarize findings, risks, and options
	•	Output: decision memo
	•	Gate: none
	•	Failure: retry

⸻

7. Failure Handling (Global)
	•	infrastructure failures → retry
	•	tool non-convergence → mark as weak evidence
	•	repeated failures → confidence penalty + human review

⸻

8. Replay & Audit

All workflows must:
	•	be replayable from stored manifests
	•	produce identical results under same conditions
	•	log approval decisions

⸻

9. Exit Criteria (v1)

Workflows v1 are complete when:
	•	all core workflows execute end-to-end
	•	approval gates enforce correctly
	•	failures handled deterministically

⸻

End of BMR-WORKFLOWS.md