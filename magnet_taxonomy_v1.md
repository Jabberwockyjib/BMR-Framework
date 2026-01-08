# Magnet R&D Problem Taxonomy (v1)
_Date: 2026-01-05_

This taxonomy defines **what problems we solve**, **which physics are required**, and **where AI agents intervene**.
It is designed explicitly to support **agentic orchestration of simulation, ML, and optimization workflows**.

---

## 0. Cross-cutting constraints
- Cost (materials + processing)
- Mass / volume envelope
- Operating temperature & transients
- Corrosion & environment
- Manufacturability & yield
- Reliability & safety margins

---

## 1. Intrinsic magnetic materials (atomic scale)
**Goals**
- Maximize Mₛ, MAE, temperature stability
- Reduce rare-earth content
- Ensure phase stability

**Physics**
- Spin-polarized DFT
- SOC for MAE
- Formation energy / convex hull

**KPIs**
- Mₛ, MAE (K₁)
- Magnetic moment
- ΔE to hull

**Tools**
- Quantum ESPRESSO
- CP2K
- ASE, pymatgen, spglib
- AiiDA / atomate2

---

## 2. Microstructure & coercivity engineering
**Goals**
- Increase Hc without sacrificing Br
- Control switching distributions

**Physics**
- Micromagnetics
- Domain wall pinning
- Demagnetizing fields

**KPIs**
- Hc, Br, (BH)max
- Switching field distribution

**Tools**
- Mumax³
- OOMMF
- ML surrogates

---

## 3. Manufacturing → property linkage
**Goals**
- Predict yield & performance from process variables

**KPIs**
- Property distributions
- Cost vs performance

**Tools**
- Data-driven ML
- Micromagnetics (optional)

---

## 4. Magnet assemblies & field shaping
**Goals**
- Field strength, uniformity, force control

**Physics**
- Magnetostatics
- Nonlinear μ

**KPIs**
- Field homogeneity
- Force / torque

**Tools**
- Elmer FEM
- GetDP
- FEMM
- Gmsh, ParaView

---

## 5. Motors & generators
**Goals**
- Torque density
- Efficiency
- Loss reduction

**Physics**
- Transient EM
- Eddy currents
- Core losses

**KPIs**
- Torque, ripple
- Efficiency maps

**Tools**
- Elmer FEM
- FEMM (2D screening)

---

## 6. Magneto-thermal & demagnetization risk
**Goals**
- Prevent thermal runaway
- Define derating envelopes

**Physics**
- EM → thermal coupling
- Temperature-dependent Hc

**KPIs**
- Hot-spot temperature
- Demag margin

**Tools**
- Elmer FEM
- OpenFOAM
- ParaView

---

## 7. Structural integrity
**Goals**
- Survive centrifugal, shock, thermal stress

**Physics**
- Structural FEA
- Thermal expansion mismatch

**KPIs**
- Max stress
- Safety factor

**Tools**
- CalculiX
- Code_Aster

---

## 8. Uncertainty & robustness
**Goals**
- Quantify sensitivity
- Reduce testing burden

**Tools**
- Bayesian optimization
- Active learning
- AiiDA provenance

---

## 9. Inverse design & trade-space exploration
**Goals**
- Meet targets with minimal cost & risk

**Tools**
- Surrogate models
- Multi-fidelity optimization
