# BMR Wedge Adapter — [Domain Name] (v1.0)

> **Template:** Copy this file to create a new domain adapter. Replace all `[bracketed text]` with domain-specific content.

---

## 1. Purpose

This adapter defines how BMR (BMad Materials Research) is specialized for [domain] R&D, with an initial focus on:
- [Primary goal 1]
- [Primary goal 2]
- [Primary goal 3]

This adapter does not modify BMR core behavior.
It supplies domain-specific objectives, tools, constraints, ontologies, and fidelity rules.

**Problem taxonomy:** [Reference to domain taxonomy if available, or list key problem categories]

---

## 2. Target Application Scope

### In-scope
- [Material/system type 1]
- [Material/system type 2]
- [Optimization approach]
- [Simulation/ML methodology]

### Out-of-scope (v1)
- [Excluded area 1]
- [Excluded area 2]
- [Future extensions]

---

## 3. Objective Adapter

### 3.1 Primary Objective

[Mathematical formulation of primary objective]

```
maximize/minimize [objective_function]
```

### 3.2 Hard Constraints

All candidates must satisfy:

| Property | Constraint |
|----------|------------|
| [Property 1] | [Constraint expression] |
| [Property 2] | [Constraint expression] |
| [Property 3] | [Constraint expression] |

### 3.3 Soft Objectives (Trade-offs)

| Objective | Weight | Direction |
|-----------|--------|-----------|
| [Soft objective 1] | [0.0-1.0] | minimize/maximize |
| [Soft objective 2] | [0.0-1.0] | minimize/maximize |

---

## 4. Property Model Adapter

### 4.1 Key Properties

| Property | Unit | Source |
|----------|------|--------|
| [Property 1] | [unit] | [ML/simulation/experiment] |
| [Property 2] | [unit] | [ML/simulation/experiment] |
| [Property 3] | [unit] | [ML/simulation/experiment] |

### 4.2 Simulation Tools

| Tool | Physics Domain | Fidelity |
|------|----------------|----------|
| [Tool 1] | [Domain] | [L-level] |
| [Tool 2] | [Domain] | [L-level] |

### 4.3 Fidelity Ladder (L-Levels)

| Level | Method | Description | Typical Cost |
|-------|--------|-------------|--------------|
| L0 | Literature heuristics | [Description] | Negligible |
| L1 | ML screening | [Description] | Low |
| L2 | [Method] | [Description] | Low-Medium |
| L3 | [Method] | [Description] | Medium |
| L4 | [Method] | [Description] | High |
| L5 | [Method] | [Description] | High |

**Note:** L-levels define method fidelity across this wedge. Within each L-level, implementations may use T-tiers (T0-T3) for mesh/resolution fidelity. See `BMR-CORE.md` Section 8.2 for the complete fidelity hierarchy definition.

Promotion requires:
- uncertainty below threshold
- decision impact justification
- cost justification

---

## 5. Experiment Adapter

### 5.1 What Counts as an Experiment
- [Experiment type 1]
- [Experiment type 2]
- [Experiment type 3]

### 5.2 Experiment Costs

| Experiment Type | Typical Cost (USD) | Typical Runtime |
|-----------------|-------------------|-----------------|
| [Type 1] | [cost] | [time] |
| [Type 2] | [cost] | [time] |

---

## 6. DOE Rules (Domain-Specific)

### 6.1 Variable Ranges

| Variable | Min | Max | Step |
|----------|-----|-----|------|
| [Variable 1] | [min] | [max] | [step] |
| [Variable 2] | [min] | [max] | [step] |

### 6.2 Constraints on Design Space
- [Constraint 1: e.g., certain combinations are invalid]
- [Constraint 2]

### 6.3 Default Exploration Strategy
- [Strategy description]

---

## 7. Hypothesis Adapter

### 7.1 Mechanism Classes

| ID | Class | Description |
|----|-------|-------------|
| M1 | [Class name] | [Description] |
| M2 | [Class name] | [Description] |
| M3 | [Class name] | [Description] |

### 7.2 Required Hypothesis Elements
- Mechanism from defined classes
- Directional prediction
- Falsification criterion
- Required fidelity level

---

## 8. Skeptic / QA Rules (Domain-Specific)

### 8.1 Known Failure Modes
- [Failure mode 1]
- [Failure mode 2]

### 8.2 Domain-Specific Red Flags
- [Red flag 1: e.g., implausible values]
- [Red flag 2]

### 8.3 Required Validations
- [Validation 1]
- [Validation 2]

---

## 9. Confidence Score Inputs

Domain-specific factors affecting confidence:

| Factor | Weight | Description |
|--------|--------|-------------|
| [Factor 1] | [weight] | [Description] |
| [Factor 2] | [weight] | [Description] |

---

## 10. Reporting & IP

### 10.1 Key Metrics for Reports
- [Metric 1]
- [Metric 2]

### 10.2 IP-Relevant Outputs
- [Output type 1]
- [Output type 2]

---

## 11. v1 Exit Criteria

This adapter v1 is complete when:
- [ ] [Criterion 1]
- [ ] [Criterion 2]
- [ ] [Criterion 3]

---

End of BMR-ADAPTER-TEMPLATE.md
