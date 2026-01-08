# SAL — Scientific Action Language
Version: 1.0  
Status: Draft (Developer-Ready)  

---

## 1. Purpose

Scientific Action Language (SAL) is a **formal, machine-enforceable language** for conducting closed-loop scientific research with AI agents.

SAL exists to:
- Prevent untestable speculation
- Enforce physics-grounded experimentation
- Enable multi-fidelity, multi-physics orchestration
- Accumulate irreversible, executable scientific knowledge

SAL replaces informal “hypotheses” with **testable claims** and enforces a single canonical loop:

```
CLAIM → INTERVENTION → TEST → UPDATE → (new CLAIM)
```

No step is optional.  
No step may be skipped.  
No step may reference entities outside the system ontology.

---

## 2. Core Concepts

### 2.1 Claim
A **Claim** is a falsifiable statement about how changing one or more variables affects a measurable property.

Claims are not ideas or intuitions — they are **contracts with reality**.

### 2.2 Intervention
An **Intervention** specifies exactly what variable is changed, by how much, and under what controls.

If a variable cannot be represented in the system, it **cannot be intervened upon**.

### 2.3 Test
A **Test** is a physics-grounded evaluation of a claim at a defined fidelity and budget.

Tests map to **physics nodes**, not tools.

### 2.4 Update
An **Update** records how system beliefs, models, and constraints change after a test.

If no belief changes, the test is considered invalid.

---

## 3. SAL Core Objects (Schemas)

### 3.1 CLAIM

```yaml
claim_id: C-000001
type: causal | comparative | bounding
statement: >
  Reducing grain_size_mean from 5 µm to 3 µm
  increases coercivity Hc by at least 10%
  at 120 C.
target_property:
  name: micromagnetics.Hc_Apm
expected_direction: increase
expected_magnitude:
  min_pct: 10
assumptions:
  - Ms_constant: true
  - K1_constant: true
  - temperature_C: 120
valid_domain:
  material_class: NdFeB
  grain_size_mean_um: [1.5, 6.0]
confidence_prior: 0.35
status: proposed | active | retired
```

---

### 3.2 INTERVENTION

```yaml
intervention_id: I-000001
claim_id: C-000001
action:
  variable: microstructure.grain_size_mean_um
  from: 5.0
  to: 3.0
control_variables:
  temperature_C: 120
  Ms_constant: true
  K1_constant: true
intervention_type: parametric | structural | compositional
```

---

### 3.3 TEST

```yaml
test_id: T-000001
claim_id: C-000001
physics_node_id: P2_coercivity_mechanisms
fidelity:
  tier: T1
  implementation_id: I_mumax_gpu_sweep
inputs:
  microstructure:
    grain_size_mean_um: 3.0
    grain_size_std_um: 0.6
  temperature_C: 120
outputs_required:
  - micromagnetics.Hc_Apm
uncertainty_target_pct: 15
budget:
  max_wall_time_s: 600
  max_cost_units: 1.0
status: queued | running | completed | failed
```

---

### 3.4 UPDATE

```yaml
update_id: U-000001
test_id: T-000001
observations:
  micromagnetics.Hc_Apm:
    baseline: 8.2e5
    observed: 9.1e5
    delta_pct: 11.0
uncertainty:
  confidence: 0.78
claim_outcome: supported | weakened | refuted
posterior_updates:
  claim_confidence: 0.35 -> 0.71
knowledge_artifacts:
  - type: executable_rule
    content: >
      For NdFeB at 120 C,
      grain_size_mean_um <= 3.5
      required for Hc >= 9e5 A/m
```

---

## 4. Execution Rules

1. No free hypotheses
2. No tool-driven exploration
3. No silent failures
4. No learning without update
5. Bounded creativity

---

## 5. Definition of Done

SAL v1 is complete when the system can:
- Accept or reject claims automatically
- Prevent untestable interventions
- Execute physics-grounded tests
- Update beliefs quantitatively
- Propose the next grounded claim

---

End of document.
