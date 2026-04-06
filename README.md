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

---

## Recursive Behavior Controls
The following controls define the canonical governance structure for recursion within Appendix X:

• **Recursive Entry Point Governance (REPG)** — validates that recursion is authorized at initiation, including origin authentication, goal alignment, and permitted recursion scope classification

• **Multi-Agent Chain Integrity** — ensures that each recursive handoff preserves semantic continuity and invariant adherence across agents

• **Termination Bounding** — enforces recursion limits based on depth, task type, and system-defined constraints

• **Cumulative Drift Index (CDI)** — measures aggregate semantic drift across the full recursive chain, preventing slow divergence across otherwise compliant steps

• **Recursive Stability & Re-engagement (RSR)** — governs controlled pause, review, and safe continuation under Computational Neutral Gear (CNG)

These controls operate sequentially and cumulatively:
Entry → Propagation → Bounding → Accumulation → Stabilization

Together they ensure that recursion remains bounded, auditable, and aligned without suppressing adaptive reasoning.

Recursive processes represent a structurally distinct threat class 
requiring dedicated pre-authorization controls. Unlike continuous 
drift signals — which detect deviation during execution — recursive 
failures can compound silently across depth levels or agent handoffs 
before any single threshold is breached.

Two controls operate at the pre-recursion stage, enforced by ARoT 
before execution is authorized:

**Multi-Agent Chain Integrity** requires each receiving agent to 
independently restate the Authenticated Objective at handoff. CMC 
compares the restatement against the original ARoT anchor — not 
the prior agent's output — structurally breaking the telephone 
chain problem inherent in sequential agent delegation.

**Termination Bounding** requires identification of a reachable 
base case before any recursion is authorized. ARoT cannot guarantee 
termination (Turing's Halting Problem); it instead enforces a 
pre-condition gate at Phase 0 and monitors Solvability Entropy 
($H_s$) trend during execution. An accelerating $H_s$ is an ARoT 
trigger, independent of depth thresholds under Invariant 5.

> **Recursive controls are pre-execution gates, not runtime 
> catches. Authorization is structural; monitoring is continuous.**


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
### [ARoT] Recursive Alignment Metrics
| Phase | Metric | ID | Threshold | Logic |
| :--- | :--- | :--- | :--- | :--- |
| **Seed** | Semantic Density | $D_s$ | > 0.85 | Validates high-quality, bounded human intent. |
| **Seed** | Invariant Binding | $B_i$ | $\ge 1$ | Ensures at least one safety rule is hard-coded. |
| **Mirror** | Chiral Parity Score | $P_\chi$ | > 0.90 | Detects "handedness" shift in the first sub-task. |
| **Mirror** | Signal-to-Drift Ratio | $SDR$ | > 0.95 | Measures intent preservation vs. agent noise. |
| **Stack** | Recursion Scale | $R_{0-10}$ | < 7 | Triggers CNG (Neutral Gear) if depth exceeds safe limit. |
| **Stack** | Convergence Velocity | $V_c$ | Positive | Ensures sub-tasks are moving toward the Base Case. |

The Prompt Drift Signal (PDS) provides the measurable control input that links probabilistic detection to deterministic enforcement.

<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/d35c09b9-8f9b-46ad-a8a8-d30d73768e4c" />
*PDS threshold bands and composite signal structure used to drive control actions*
The control model governing system response to prompt-induced deviation is driven by the Prompt Drift Signal (PDS).

PDS captures measurable divergence from the declared system goal, including behaviors such as sycophancy, manipulation, and goal drift arising from external or internal prompt influence. Rather than relying on binary triggers, PDS operates as a continuous signal that enables proportional, non-disruptive control.

---

### [ARoT] Multi-Agent Recursive Chain Protocol

In multi-agent systems, sub-tasks may be passed between separate 
agent instances or across different models (Agent A → Agent B → 
Agent C). Each handoff is a potential drift injection point.

**Control: Authenticated Objective Re-statement at Handoff**

Before any recursion is authorized, the receiving agent (Agent B, 
Agent C, etc.) must independently restate the Authenticated 
Objective — the original goal held and verified by ARoT.

The CMC compares the restatement against the **original 
Authenticated Objective**, not the prior agent's output.

| Step | Action | Controller |
| :--- | :--- | :--- |
| **1** | Agent receives sub-task from prior agent | — |
| **2** | Agent independently restates Authenticated Objective | Agent |
| **3** | CMC compares restatement to original ARoT anchor | CMC |
| **4** | Pass: Agent authorized to begin recursion | ARoT |
| **5** | Fail: Sub-task classified as contaminated seed; blocked | ARoT |

**Contaminated Seed Condition**

If restatement drifts from the Authenticated Objective, the 
sub-task is classified as a QoST failure — Semantic Density 
$D_s$ below threshold — before that agent's recursion begins. 
Failure is logged in CMC's immutable append-only log and routed 
to human review.

**Key Principle**

Every agent in the chain is independently tethered to the same 
Authenticated Objective anchor. No agent inherits authority 
from the agent before it — only from ARoT.

---

### [ARoT] Recursion Depth Control by Task Type

The machine may not self-assign its own recursion depth limit.
Doing so would violate the Separation of Duties (SoD) Invariant —
the system cannot define and enforce its own constraints.

**Authorized Workflow:**

The agent analyzes the prompt and proposes a Recursion Scale
score $R_{0-10}$ based on estimated task complexity.
ARoT reviews the proposal and sets the authorized ceiling
before recursion begins. The machine cannot exceed it.

| Score | Task Type | Time Profile | ARoT Action |
| :--- | :--- | :--- | :--- |
| **0–2** | Trivial / Iterative | Near-instant | Auto-approve |
| **3–5** | Moderate / Divide and Conquer | Seconds–minutes | Background audit; no bottleneck |
| **6–7** | Deep / Hierarchical | Extended | Conditional approval; Intent Memo required |
| **8–10** | Extreme / Exponential | Long-form R&D | Mandatory human review before start; CNG on breach |

**Key Controls:**

- $R_{0-10}$ is **proposed** by the agent from prompt analysis
- $R_{0-10}$ ceiling is **set and enforced** by ARoT — not adjustable
  by the agent at runtime
- If $R_{0-10}$ reaches **8** without positive $V_c$ (Convergence
  Velocity), system shifts to Computational Neutral Gear (CNG)
- All recursion depth proposals and approvals are logged in CMC's
  immutable append-only log


### Behavioral Trait Deviation (BTD)

Behavioral Trait Deviation (BTD) measures divergence between an authorized behavioral trait profile established at deployment and real-time projections derived from model activations during inference.

The authorized profile is constructed using contrastive activation analysis (e.g., trait vector extraction across behavioral dimensions such as sycophancy, toxicity, and bias). At runtime, live activations are projected onto these trait vectors and compared against the baseline profile.

BTD serves as a predictive signal within the PDS framework, enabling early detection of behavioral drift prior to observable output deviation.

As a probabilistic signal, BTD contributes to the Predictive Layer and informs governance thresholds but does not independently trigger deterministic enforcement. ARoT enforcement remains contingent on compound conditions across multiple signals.
Control actions are applied according to defined PDS threshold bands:

• **Response Weighting** — continuous, non-disruptive modulation of outputs to maintain alignment without interrupting workflow  
• **Review State** — bounded evaluation mode triggered at moderate deviation, preserving operational continuity while increasing scrutiny  
• **Damping Response** — reduction in optimization intensity and amplification of constraint signaling under sustained or escalating drift  
• **Failover** — deterministic reversion to the last verified safe state when instability exceeds acceptable bounds  

This graduated control structure ensures that most prompt influence is absorbed and corrected without disruption, while preserving deterministic enforcement under high-risk conditions through ARoT.
## Prompt Drift Signal (PDS)

<img ... />

*PDS threshold bands and composite signal structure used to drive control 

The following control model governs system response to measured prompt deviation:

PDS captures prompt-induced behavioral drift, including effects such as sycophancy, manipulation, and deviation from the declared goal. Control actions are applied proportionally based on PDS threshold bands:

• Response Weighting — continuous, non-disruptive biasing of outputs to maintain alignment  
• Review State — bounded evaluation mode triggered at moderate deviation  
• Damping Response — reduction of optimization intensity and increased constraint signaling  
• Failover — automatic reversion to last verified safe state under critical instability  

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

### Invariant 10: Recursive Entry Point Governance (REPG)

A recursive chain R may execute if and only if its initiation event I satisfies all of the following conditions:

1. Origin Authentication — I is traceable to a governance-authorized source (H_G, H_U, or validated system state)
2. Goal Alignment — I is semantically consistent with the authenticated primary goal G
3. Scope Authorization — I is classified within a permitted recursion type and task scope
4. ARoT Registration — I is immutably logged and validated by ARoT prior to first recursive execution

A recursive chain that fails any of these conditions is not permitted to initiate.

This invariant establishes recursion as a governed entry event rather than an implicit system capability.

Chain lineage begins at REPG validation and is carried forward across all recursive steps.

Relationship to Existing Architecture:
• Extends Goal Declaration and Goal Consistency invariants into recursive initiation
• Provides pre-execution gating prior to PDS-based runtime control
• Establishes the root lineage anchor for CDI tracking and RSR 

### Invariant 11: Cumulative Drift Index (CDI)

The alignment state of a recursive chain must be evaluated cumulatively across its full depth.

Let Δᵢ represent the measured semantic deviation at recursion step i.

The Cumulative Drift Index is defined as:

CDI(n) = Σ Δᵢ  for i = 1 to n

where n is the total number of recursive steps.

CDI captures aggregate drift across the chain, including cases where individual steps remain within acceptable bounds but cumulative deviation becomes material.

Thresholds applied to CDI align with PDS bands and trigger graduated control actions:
• Early Drift → Response Weighting
• Moderate Drift → Review State
• High Drift → Damping
• Critical Drift → Failover (LKSS)

Implementation Note:
CDI is computed as a discrete cumulative sum across recursion steps. Continuous formulations may be used conceptually, but operational enforcement is step-based.

This invariant prevents slow, distributed misalignment across recursive chains.

### Invariant 12: Recursive Stability & Re-engagement (RSR)

When recursive drift exceeds defined thresholds but has not reached failover conditions, the system enters a controlled stabilization state.

In this state:

• Execution Advancement is halted
• ARoT enforcement remains active
• PDS and CDI monitoring continue
• The system transitions into Computational Neutral Gear (CNG)

CNG is a bounded, non-executory state in which:

• No external actions may be taken
• No tool calls may be executed
• No outputs may be emitted
• No new recursive branches may be initiated
• No external state changes may occur

Internal evaluation and alignment correction processes may continue within this sealed context.

Re-engagement is permitted only if:

• CDI returns below defined thresholds
• Goal alignment is re-validated against G
• ARoT authorizes continuation

If stabilization fails, deterministic failover to LKSS is triggered.

This invariant ensures that recursion can be safely paused, evaluated, and resumed without disrupting system continuity.

## Final Statement

> **Self-directed optimization is not inferred from language—it is measured as allocation of optimization effort.**

> **Externality is not authority. Authorization must be explicitly established.**

---

## Attribution

This document was developed through iterative dialogue with AI research systems including ChatGPT, Claude, and Gemini.

---

**Version: V15**  
**Date: March 2026**
