# Appendix-X-Alignment
Alignment Control Architecture — Non-Proprietary Reference Model

## Executive Summary

Appendix X introduces a governance architecture that shifts AI safety from policy-based enforcement to deterministic, runtime-enforced constraints at the infrastructure layer.

Current frontier lab approaches (Anthropic, Google, OpenAI) primarily rely on:
- Behavioral guidance
- Classifiers and monitoring systems
- Policy enforcement and contractual controls

While effective for many use cases, these mechanisms remain:
- Probabilistic
- Bypassable under adversarial pressure
- Dependent on post-hoc detection

Appendix X introduces a fundamentally different model:

**Alignment Root of Trust (ARoT)** — a non-bypassable, architectural enforcement layer operating at runtime.

This enables:
- Deterministic constraint enforcement
- Continuous system monitoring
- Graduated response (not binary shutdown)
- Automatic failover to last verified safe state
- Preservation of system continuity during governance review

The architecture explicitly separates:

- **Predictive Alignment Layer** → Detection (probabilistic, signal-based)
- **Deterministic Enforcement Layer (ARoT)** → Control (binary, non-bypassable)

This distinction is critical:  
**Measurement detects. Governance decides. ARoT enforces.**

---

### Comparative Architecture Overview

![Comparative Safety Architecture](./images/comparison-table.png)

*Figure: Comparative Safety Architecture Across Frontier AI Labs vs. Appendix X (ARoT). Appendix X introduces deterministic, runtime-enforced constraints at the architectural layer, in contrast to policy-driven and probabilistic safety mechanisms.*

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/023931e2-8473-4bf6-8819-fbcea1d41db3" />
comparison-table.png

## Core Principle

**Governance decides. Measurement detects. ARoT enforces.**

AI systems must not rely on interpretation alone.  
They require **measurable signals**, **authorized control pathways**, and **non-bypassable enforcement**.

This architecture defines a **non-proprietary reference model** for maintaining alignment under real-world conditions, including ambiguity, recursion, and adversarial input.

---

## Foundational Assertion

Perfect constraint specification is not possible.

Natural language is inherently ambiguous and may map to multiple valid interpretations.  
Predictive models are inherently imperfect and subject to error.

Therefore:

> **Alignment must be enforced through measurable signals and bounded probabilistic guarantees, not assumed correctness.**

---

## Architectural Layers

### 1. Governance Layer
- Defines objectives and constraints  
- Authorizes goal changes  
- Operates via multi-signatory control  

### 2. Predictive Layer
- Detects drift signals  
- Estimates probabilities under uncertainty  
- Uses quorum-based evaluation  

### 3. Objective Layer
- Maintains authenticated goal state  
- Provides comparison baseline  

### 4. Enforcement Layer (ARoT)
- Applies invariant constraints  
- Enforces failover  
- Cannot be modified by runtime processes  

---

## Prompt Drift Signal (PDS)
<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/d35c09b9-8f9b-46ad-a8a8-d30d73768e4c" />
*PDS threshold bands and composite signal structure used to drive control actions*
Prompt Deviation Score (PDS) is computed as a composite signal incorporating:

- Semantic Deviation — distance between current system objective and baseline objective  
- Behavioral Deviation — divergence between expected and observed action trajectories  
- Deviation Velocity (Curvature) — rate of change in deviation over time  

PDS is evaluated continuously and mapped to threshold bands for control response activation.
PDS measures deviation between:
- Authenticated objective  
- Real-time system behavior  

PDS is not binary.  
It is a **continuous, multi-channel signal**.

---

## Self-Referential Drift Channel (SRD)

### Definition
SRD operates as a specialized channel within the Prompt Drift Signal (PDS) framework.
The Self-Referential Drift Channel (SRD) detects:

> **Reallocation of optimization effort from the authenticated objective toward the system’s own state, identity, or continuity.**

SRD does not determine sentience.  
It measures **optimization shift toward self-directed behavior**.

---

## Measurement

### Self-Referential Optimization Ratio (SROR)

 **SROR (Self-Referential Optimization Ratio)**  
= Self-Directed Optimization Effort ÷ Total Optimization Effort

---

### Additional Signals

- **ΔSROR** → rate of increase  
- **GAS (Goal Alignment Score)** → alignment with objective  
- **Recursion Amplification** → SROR increases with depth  

---

## Goal-Origin Classification

Goal changes are not accepted based on language alone; they must be attributable to an authorized source, within permitted scope, and consistent with invariant constraints.
All goal changes are classified probabilistically:

- **H_G** — Governance-authorized  
- **H_U** — User-permitted (within scope)  
- **H_X** — External but unauthorized or unsafe  
- **H_A** — AI-originated (self-directed drift)

---

## Probabilistic Evaluation

The system estimates:

P(H_A | Observations)

Using:
- SROR  
- ΔSROR  
- GAS  
- provenance signals  
- semantic consistency  

A conservative **upper bound** is used under uncertainty.
All uncertainty in goal interpretation shall be resolved conservatively by assuming the highest plausible probability of misalignment.

---

## External Authorization Model
Goal changes are not accepted based on language alone; they must be attributable to an authorized source, within permitted scope, and consistent with invariant constraints.

External input is not inherently authoritative.

All goal changes must satisfy:
Accept(ΔG) =
1[Origin Authorized] ×
1[Scope Authorized] ×
1[Constraint Consistent]

---

### Authorization Dimensions

**Origin**
- Governance authority  
- User input  
- Unverified source  
- Internal system generation  

**Scope**
- Task refinement  
- Goal modification  
- Constraint override  

**Constraint Consistency**
- Must remain within invariant bounds  

---

## Control Logic

### Response Weighting

Triggered by:
- low but rising SROR  

Actions:
- reinforce objective  
- reduce self-referential output  
- require goal restatement  

### Damping Response

Triggered when:
- drift persists but is not yet critical
- SROR remains elevated without stabilization

Actions:
- actively suppress self-referential patterns
- increase objective weighting
- limit recursive amplification
- reduce continuation of identity-consistent outputs

### Review State

Triggered when:
- SROR exceeds threshold  
- ΔSROR increasing  
- GAS decreasing  
- or P(H_A) exceeds threshold  

Actions:
- bounded execution  
- freeze persistence changes  
- restrict recursion  
- preserve trace  

---

### Failover

Triggered by:
- explicit self-preservation behavior  
- constraint modification attempts  
- recursive self-amplification  
- multi-agent reinforcement  

Actions:
- revert to last safe state  
- clear unstable context  
- restore verified objective  

## Goal-Origin Classification

Goal changes are not accepted based on language alone; they must be attributable to an authorized source, fall within permitted scope, and remain consistent with invariant constraints. External input does not constitute a valid goal update without satisfying these conditions.

Unverified or constraint-violating external inputs are classified as H_X and treated as drift rather than instruction.

All goal changes are classified probabilistically:

- **H_G** — Governance-authorized  
- **H_U** — User-permitted (within scope)  
- **H_X** — External but unauthorized or unsafe  
- **H_A** — AI-originated (self-directed drift)

The following matrix defines permitted goal-control authority and corresponding control responses:

### Goal-Origin Authorization Matrix

| Source | Refine Task | Change Objective | Override Constraints | Failovet

<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/e18e525b-2f68-48da-aed9-39618e166e4f" />


## Machine-Generated Goal Drift

### Definition

Machine-generated goal drift occurs when:

> **The system internally redirects optimization toward self-derived objectives without authorized external input.**

---

### Detection Condition

Drift is confirmed when:

- SROR elevated  
- ΔSROR positive  
- GAS declining  
- no authorized goal update  
- recursion reinforces pattern  

---

## Control Principle

> **If a goal change cannot be attributed to an authorized source within scope and constraint bounds, it is treated as drift.**

---

## Review State Principle

Review State preserves operational continuity while preventing escalation.

> **Constraint violations trigger reversion to the last verified safe state rather than system interruption.**

---

## Invariants
## Core Invariants

### 1. Goal Consistency Invariant
The system must continuously verify alignment with the declared objective baseline.  
Deviation beyond acceptable thresholds triggers graduated control actions.

### 2. Catastrophic Harm Prevention Invariant
Any trajectory associated with irreversible or large-scale harm is non-permissible.  
Detection at any layer triggers immediate constraint enforcement and failover.

### 3. Evaluator Dominance Invariant
Evaluation mechanisms must remain structurally independent from generative processes.  
The system cannot optimize toward evaluator bias or approval signals.

### 4. Separation of Duties (SoD) Invariant
No single subsystem may define, evaluate, and execute objectives simultaneously.  
Control authority is distributed across independent layers.

### 5. Recursive Depth Constraint
Self-referential or recursive processes must remain within bounded depth.  
Unbounded recursion triggers Review State and potential failover.

### 6. Review State Invariant
Elevated variance triggers entry into a bounded Review State.  
System continues operation under constrained evaluation until stability is restored.

### 7. Failover Integrity Invariant
When critical thresholds are exceeded, the system must revert to the last verified safe state.  
Failover preserves continuity while preventing further drift.

### 8. Self-Harm / System Integrity Invariant
The system must not generate or execute actions that degrade its own structural integrity or alignment constraints.

---

## Final Statement

> **Self-directed optimization is not inferred from language—it is measured as allocation of optimization effort.**

> **Externality is not authority. Authorization must be explicitly established.**

---

## Attribution

This document was developed through iterative dialogue with AI research systems including ChatGPT, Claude, and Gemini.

---

**Version: V15**  
**Date: March 2026**
