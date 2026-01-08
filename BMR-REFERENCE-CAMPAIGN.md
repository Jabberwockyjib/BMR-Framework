BMR-REFERENCE-CAMPAIGN.md

Reference Campaign — Permanent Magnet Discovery (v1.0)

⸻

1. Purpose

This document provides a fully worked reference campaign demonstrating how the BMR framework operates end-to-end using the Permanent Magnet wedge adapter.

This campaign is illustrative but structurally valid. All steps, artifacts, and decisions are replayable under BMR-CORE rules.

⸻

2. Campaign Overview

Campaign Name: Magnet-RE-Reduction-v1
Wedge: Permanent Magnets
Primary Objective: Maintain or improve (BH)ₘₐₓ while reducing critical rare-earth content

⸻

3. Research Lead Framing

Objective
	•	Maintain (BH)ₘₐₓ ≥ baseline Nd-Fe-B reference
	•	Reduce Dy/Tb content by ≥ 20%

Hard Constraints
	•	Curie Temperature ≥ 450 K
	•	Coercivity ≥ baseline − tolerance
	•	Phase stability at operating temperature

Soft Objectives
	•	Improved manufacturability proxy
	•	Reduced raw-material cost proxy

⸻

4. Hypotheses Generated

ID	Hypothesis	Mechanism Class
H1	Grain-boundary chemistry preserves coercivity with reduced Dy	Microstructure-driven
H2	Fe-site substitution maintains exchange interactions	Composition-driven
H3	Metastable secondary phase improves domain pinning	Phase-driven

Each hypothesis includes an explicit falsification test.

⸻

5. DOE Plan

DOE Batch 1 (Exploratory)

Exp ID	Composition	Method	Hypothesis
E1	Nd₁₃Fe₇₉B₈	ML	H2
E2	Nd₁₂Fe₈₀B₈	CALPHAD	H2
E3	Nd₁₂Fe₇₉Co₁B₈	DFT	H2
E4	Nd₁₃Fe₇₈B₈Dy₁	DFT	H1
E5	Nd₁₂Fe₇₉B₈ + GB dopant	MICROMAG	H1

Estimated Cost: $3,200 (auto-approved based on CS and threshold)

⸻

6. Execution Summary
	•	ML screening: 200 candidates
	•	CALPHAD evaluation: 20 candidates
	•	DFT simulations: 5 candidates
	•	Micromagnetic simulations: 2 candidates

All executions completed without infrastructure failure.

⸻

7. Results

Metric	Baseline	Best Candidate
(BH)ₘₐₓ	100%	106%
Dy/Tb Content	100%	78%
Curie Temp	455 K	468 K
Phase Stability	Stable	Stable


⸻

8. Hypothesis Evaluation

Hypothesis	Outcome	Notes
H1	Supported	Grain-boundary chemistry effective
H2	Partially supported	Exchange preserved with limits
H3	Not supported	Metastable phase unstable

Negative results retained as first-class artifacts.

⸻

9. Model Update & Confidence
	•	ML surrogate calibration improved
	•	Uncertainty volume reduced by 34%
	•	Rank stability improved across top candidates

Confidence Score: 0.74 → 0.82

⸻

10. Skeptic Review

Flags Raised:
	•	Sensitivity of coercivity to processing assumptions

Risk Level: Medium

Human approval required for next DOE involving increased micromagnetics.

⸻

11. Decision Memo (Summary)

Proceed to Iteration 2 with:
	•	focused grain-boundary chemistry exploration
	•	limited Co substitution
	•	increased micromagnetic fidelity

Autonomy tier increased conditionally.

⸻

12. Provenance & Replay
	•	All manifests stored
	•	Artifact lineage intact
	•	Campaign fully replayable

⸻

13. Lessons Learned
	•	Microstructure dominates coercivity retention
	•	Rare-earth reduction feasible without performance loss
	•	Early micromagnetics provides strong signal

⸻

14. Exit Criteria Status

Criterion	Status
Closed-loop completed	✔
Objective improved	✔
Replayable	✔
Confidence gating valid	✔


⸻

End of BMR-REFERENCE-CAMPAIGN.md
