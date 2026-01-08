BMR-MAGNETS-ADAPTER.md

BMR Wedge Adapter — Permanent Magnet Materials (v1.0)

⸻

1. Purpose

This adapter defines how BMR (BMad Materials Research) is specialized for permanent magnet R&D, with an initial focus on:
	•	improving magnetic performance
	•	reducing critical rare-earth content
	•	maintaining manufacturability and thermal stability

This adapter does not modify BMR core behavior.
It supplies domain-specific objectives, tools, constraints, ontologies, and fidelity rules.

**Problem taxonomy:** This adapter addresses categories 1-6 and 9 from [magnet_taxonomy_v1.md](magnet_taxonomy_v1.md) (atomic materials, microstructure, manufacturing, assemblies, motor optimization, thermal management, and inverse design).

⸻

2. Target Application Scope

In-scope
	•	Nd-Fe-B–based systems (including reduced Dy/Tb variants)
	•	Fe-based rare-earth–lean or rare-earth–free systems
	•	Composition + microstructure–driven performance optimization
	•	Simulation-first and ML-augmented discovery

Out-of-scope (v1)
	•	Soft magnets
	•	Fully process-integrated industrial scale-up
	•	Long-term corrosion / aging studies
	•	Full synthesis automation (lab adapter later)

⸻

3. Objective Adapter

3.1 Primary Objective

Maximize energy product:

maximize (BH)ₘₐₓ

⸻

3.2 Hard Constraints

All candidates must satisfy:

Property	Constraint
Curie Temperature (Tc)	Tc ≥ Tc_min
Coercivity (Hc)	Hc ≥ Hc_min
Rare Earth Content	≤ RE_max (atomic %)
Density	≤ ρ_max
Phase Stability	Stable at operating T

Thresholds are campaign-configurable.

⸻

3.3 Soft Objectives (Penalty-Weighted)
	•	Reduction in critical rare earths (Dy, Tb)
	•	Manufacturability proxy score
	•	Raw-material cost proxy

⸻

4. Property Model Adapter

4.1 ML Models

Model	Inputs	Outputs
ML-BH	Composition + descriptors	(BH)ₘₐₓ ± σ
ML-Tc	Composition	Tc ± σ
ML-Phase	Composition	Phase stability class
ML-RE	Composition	Critical RE fraction

All models must report uncertainty.

⸻

4.2 Simulation Tools

Tool	Purpose	Fidelity
DFT	Exchange + formation energy	High
CALPHAD	Phase stability vs T	Medium
Micromagnetics	Coercivity trends	High
Physics-informed surrogate	Fast screening	Low–Medium


⸻

4.3 Fidelity Ladder (L-Levels)

L0 — Literature heuristics
L1 — ML screening
L2 — Physics-informed surrogates
L3 — CALPHAD
L4 — Targeted DFT
L5 — Micromagnetics

**Note:** L-levels define method fidelity across this wedge. Within each L-level, implementations may use T-tiers (T0-T3) for mesh/resolution fidelity. See `BMR-CORE.md` Section 8.2 for the complete fidelity hierarchy definition.

Promotion requires:
	•	uncertainty below threshold
	•	decision impact justification
	•	cost justification

⸻

5. Experiment Adapter

5.1 What Counts as an Experiment
	•	ML inference
	•	DFT calculation
	•	Phase diagram evaluation
	•	Micromagnetic simulation
	•	(Future) physical synthesis + measurement

⸻

5.2 Required Experiment Manifest

{
  "composition": {},
  "microstructure_assumptions": {},
  "method": "ML | DFT | CALPHAD | MICROMAG",
  "expected_noise": "low | medium | high",
  "estimated_cost": "usd",
  "kill_criteria": []
}


⸻

6. DOE Rules (Magnet-Specific)

6.1 Constraints
	•	composition diversity enforced
	•	near-duplicate alloys rejected
	•	uncertainty peaks prioritized
	•	exploration budget floor enforced

⸻

6.2 DOE Success Metrics

A DOE batch is successful if at least one occurs:
	•	≥ X% increase in (BH)ₘₐₓ
	•	≥ Y% RE reduction at constant performance
	•	≥ Z% uncertainty volume reduction

⸻

7. Hypothesis Adapter

7.1 Required Hypothesis Classes

Each campaign must include ≥2 classes:
	1.	Composition-driven
	2.	Microstructure-driven
	3.	Phase-driven

⸻

7.2 Hypothesis Schema

{
  "hypothesis_id": "H1",
  "mechanism": "string",
  "expected_effect": "increase | decrease | tradeoff",
  "measurable_discriminator": "property",
  "falsification_test": "experiment_id"
}


⸻

8. Skeptic / QA Rules

The Skeptic Agent must flag:
	•	extrapolation beyond training space
	•	unexplained coercivity gains
	•	unstable-phase-driven improvements
	•	repeated confirmation without falsification

If flagged:
	•	confidence score capped
	•	human approval forced

⸻

9. Confidence Score Inputs
	•	ML vs DFT agreement
	•	rank stability
	•	falsification rate
	•	phase survival rate
	•	reproducibility

⸻

10. Reporting & IP

Required Outputs per Iteration
	1.	Magnet Decision Memo
	2.	Technical Appendix
	3.	IP Trigger Report (if applicable)

⸻

11. Exit Criteria (v1)

Adapter is validated when:
	•	one closed-loop campaign completes
	•	objective improves vs baseline
	•	decisions are replayable
	•	confidence gating functions correctly
	•	cost tracking matches reality

⸻

12. Forward Compatibility

Designed to extend to:
	•	lab automation
	•	synthesis/process variables
	•	real measurement data
	•	other hard-magnet systems

⸻

End of BMR-MAGNETS-ADAPTER.md