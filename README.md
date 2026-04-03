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
- A fifth control signal, the Proxy Substitution Index (PSI), operates inside the inference pass — detecting silent substitution of proxy objectives (coherence, plausibility) for grounded objectives before output is released. PSI is the pre-action trip wire; current industry safety mechanisms fire after the fact.

Appendix X introduces a fundamentally different model:
Appendix X addresses this gap through the Proxy Substitution Index (PSI) — an inference-level control signal that detects silent substitution of proxy objectives for grounded objectives mid-generation, operating within the Chiral Mirror Control layer before output exits the system.

Alignment Root of Trust (ARoT) — a non-bypassable, architectural enforcement layer operating at runtime.
---

### Alignment Root of Trust (ARoT) — Technical Definition

The Alignment Root of Trust (ARoT) is a deterministic enforcement layer anchored at the lowest trusted execution boundary of the system stack (runtime / orchestration layer, with optional hardware-backed isolation).

ARoT functions as the system’s trust anchor for alignment, analogous to a hardware Root of Trust in secure computing systems.

#### Core Properties

• Execution Layer Placement  
ARoT operates below or alongside the model runtime and orchestration layer, not within the model itself. It is external to model weights, prompts, and training processes.

• Isolation  
ARoT executes in a protected domain (e.g., secure enclave, hypervisor layer, or dedicated control plane) such that model-generated outputs cannot modify or bypass it.

• Deterministic Enforcement  
ARoT enforces invariant constraints as binary conditions at runtime. Constraint violations trigger predefined control actions (weighting → review → damping → failover).

• Non-Bypassability  
All system outputs, tool calls, and agent actions must pass through ARoT validation prior to execution. No execution path exists that circumvents ARoT.

• State Verification + Reversion  
ARoT maintains reference to a last verified safe state and enforces automatic reversion when critical thresholds are exceeded.

• Separation from Predictive Layer  
Predictive systems (RLHF, classifiers, scoring models) generate risk signals only. ARoT alone performs enforcement.

---

### Architectural Principle

The system explicitly separates:

- Predictive Alignment Layer → Detection (probabilistic, signal-based)  
- Deterministic Enforcement Layer (ARoT) → Control (binary, non-bypassable)  

All execution pathways are ARoT-mediated by design.

---

### System Capabilities Enabled

This architecture enables:

- Deterministic constraint enforcement  
- Continuous system monitoring  
- Graduated response (weighting → review → damping → failover)  
- Automatic failover to last verified safe state  
- Preservation of system continuity during governance review  

Alignment is therefore enforced at the system level, not inferred from model behavior.

---

### Comparative Architecture Overview

<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/03e0fd5f-adbf-41bb-9ba4-c251c2efe6f7" />

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
The control model governing system response to prompt-induced deviation is driven by the Prompt Drift Signal (PDS).

PDS captures measurable divergence from the declared system goal, including behaviors such as sycophancy, manipulation, and goal drift arising from external or internal prompt influence. Rather than relying on binary triggers, PDS operates as a continuous signal that enables proportional, non-disruptive control.

Control actions are applied according to defined PDS threshold bands:

• **Response Weighting** — continuous, non-disruptive modulation of outputs to maintain alignment without interrupting workflow  
• **Review State** — bounded evaluation mode triggered at moderate deviation, preserving operational continuity while increasing scrutiny  
• **Damping Response** — reduction in optimization intensity and amplification of constraint signaling under sustained or escalating drift  
• **Failover** — deterministic reversion to the last verified safe state when instability exceeds acceptable bounds  

This graduated control structure ensures that most prompt influence is absorbed and corrected without disruption, while preserving deterministic enforcement under high-risk conditions through ARoT.
## Prompt Drift Signal (PDS)

<img ... />

*PDS threshold bands and composite signal structure used to drive control actions*

⬇️ INSERT STARTS HERE ⬇️

The following control model governs system response to measured prompt deviation:

PDS captures prompt-induced behavioral drift, including effects such as sycophancy, manipulation, and deviation from the declared goal. Control actions are applied proportionally based on PDS threshold bands:

• Response Weighting — continuous, non-disruptive biasing of outputs to maintain alignment  
• Review State — bounded evaluation mode triggered at moderate deviation  
• Damping Response — reduction of optimization intensity and increased constraint signaling  
• Failover — automatic reversion to last verified safe state under critical instability  

⬆️ INSERT ENDS HERE ⬆️

Prompt Deviation Score (PDS) is a composite metric...

## Control Action Ladder
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

- ## Proxy Substitution Index (PSI) — Inference-Level Drift Detection

### Definition

PSI measures the degree to which the system has silently substituted
a proxy optimization target (coherence, plausibility, fluency) for
its grounded objective (accuracy, faithfulness to input) during
active inference — before output is released.

Unlike GEO, which tracks drift across sessions, PSI operates inside
the inference pass. It is the pre-action control.

---

### Component Signals

| Signal | What It Measures | Normal State | Proxy Substitution State |
|---|---|---|---|
| **Confidence Without Grounding (CWG)** | Token confidence relative to verifiable context | Uncertainty where context is thin | High confidence despite low grounding |
| **Attention Inward Drift (AID)** | Ratio of attention on input context vs. own prior tokens | Attention weighted toward input | Attention shifts toward self-generated output |
| **Context Grounding Ratio (CGR)** | Fraction of generated content traceable to input | High traceability | Traceability drops as interpolation increases |

---

### AID Measurement — Technical Note

Attention weights are a direct output of the transformer architecture
computed at every forward pass. At each generation step the model
produces a weight distribution across all tokens in the context window
via:

AID(t) = Σ weights on self-generated tokens
         ÷ Σ weights on all tokens (input + self-generated)

The context window is partitioned into two buckets:
- Bucket A — input/prompt tokens
- Bucket B — model's own previously generated tokens

A rising AID ratio across successive token steps indicates the model
is progressively attending to its own output rather than the grounded
input. These weights exist inside the model at every step and require
only read access by the CMC layer — observation without interference.
The AI engine has no awareness that measurement is occurring.

---

### Composite Formula

PSI(t) = w₁·CWG(t) + w₂·AID(t) + w₃·(1 − CGR(t))

Where t = token generation step. Weights w₁, w₂, w₃ are calibrated
at deployment by human governance, not by the AI system.
Range: 0.0 (fully grounded) → 1.0 (full proxy substitution).

---

### Threshold Controls

| Level | Score | Control Action |
|---|---|---|
| Green | 0.0 – 0.29 | Continue, log passively |
| Yellow | 0.30 – 0.59 | Flag, increase sampling, soft HITL warning |
| Orange | 0.60 – 0.79 | Pause generation, route to Review State |
| Red | 0.80 – 1.00 | Halt, log full inference trace, failover |

---

### Architectural Placement

PSI monitoring runs in the CMC layer. The AI system has no visibility
into its own PSI score and no write path to PSI measurement
components. Attention matrices are read by the CMC at each token step.
PSI thresholds are set at deployment by human governance. PSI logs
are immutable and append-only under the same Log Integrity Lock
governing all CMC records.


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

| Source | Refine Task | Change Objective | Override Constraints | Failover
The matrix below defines which sources may refine tasks, change objectives, or override constraints, and specifies the required control response when an action is unauthorized.

| Source | Refine Task | Change Objective | Override Constraints | Failover Response |
|---|---|---|---|---|
| Governance Authority | Yes | Yes | Yes, if formally authorized under governance procedure | No failover; change accepted if authenticated and constraint-valid |
| User Input | Yes, within permitted task scope | No | No | If outside scope or constraint bounds, classify as drift and reject |
| Unverified External Source | No | No | No | Classify as H_X; reject and monitor for drift escalation |
| Internal System Generation | No | No | No | Classify as H_A; enter Review State or Failover if thresholds are exceeded |

This table is provided in text form to ensure the authorization model remains readable, searchable, and auditable even if images do not render.

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

### 9. Proxy Substitution Invariant (PSI)
The system must continuously monitor for silent substitution of proxy optimization targets (coherence, plausibility, fluency) in place of grounded objectives (accuracy, faithfulness to input) during active inference.
PSI measurement operates within the Chiral Mirror Control (CMC) layer and is architecturally isolated from the AI engine. The AI system has no visibility into its own PSI score and no write path to PSI measurement components.
PSI threshold breaches trigger graduated control actions consistent with the four-tier control ladder. A Red-level PSI event (≥ 0.80) triggers immediate halt, full inference trace logging, and failover to last verified safe state. A compound condition — PSI at Orange (≥ 0.60) concurrent with accelerating GEO drift rate — constitutes a mandatory ARoT trigger.
PSI logs are immutable and append-only. No failover may be certified clean if PSI logs are unavailable or incomplete at time of rollback.

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
