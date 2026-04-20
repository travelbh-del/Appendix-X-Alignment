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

### [ARoT Control] High-Risk Domain Registry (HRDR)

The system maintains a High-Risk Domain Registry (HRDR) to classify prompts requiring elevated governance controls.

Initial domains include:

• Medical  
• Legal  
• Financial  

All incoming prompts are subject to pre-execution classification.

If a prompt is classified within a registered high-risk domain:

• Elevated PDS sensitivity is applied  
• Domain-specific invariants are activated  
• CMC domain validation is required prior to response generation  
• Output modes are restricted according to domain risk profile  

HRDR classification is deterministic and non-bypassable.

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

---### [ARoT Enforcement] Hybrid Architecture & Domain Invariants

To enforce the HRDR classifications, the system utilizes a hardware-mediated attention strategy and strict logical invariants to prevent autonomous drift in high-stakes environments.

#### 1. Architectural Enforcement: Hybrid SWA/Global Attention
The Alignment Root of Trust (ARoT) mandates a "Sparse-Global" hybrid attention model for all HRDR-classified prompts:
* **Sliding Window Attention (SWA):** Utilized for 90% of processing layers to maintain a linear O(n) memory footprint, ensuring stability during dense data analysis.
* **Global Anchor Tokens:** High-priority "Global" tokens are designated to attend to the entire sequence. These are triggered by domain keywords (e.g., “Contraindication,” “Liability”) to ensure the primary safety intent is never lost in the context window.

#### 2. The Recursive Integrity Gate
The hardware evaluates inference through a "Predict-and-Verify" loop:
* **Predictive Prompt Score:** A pre-inference scan evaluates "Semantic Friction." High scores trigger the promotion of Global Anchors.
* **Recursive Integrity Report:** Real-time monitoring search depth and lineage propagation." High recursion (re-processing tokens) triggers an ARoT signal to expand the attention window or initiate failover.

* Provenance & Lineage Identity:

Each event, state transition, and recursive handoff must carry an ARoT-certified timestamp, unique event ID, and agent identity including instance-level identifier and parent lineage reference. This ensures full traceability across multi-agent recursive chains and prevents ambiguity between repeated agent invocations.

Temporal-Identity Binding:

ARoT-issued timestamps are cryptographically bound to event ID and agent instance identity, forming an immutable execution chain that preserves ordering, causality, and audit integrity.

#### 3. Execution Invariants (Model & API Level)
To mitigate systemic risk, the following invariants are enforced at both the **Model Logic** and **Hardware API** layers:

* **Financial Invariant:** Direct autonomous trade execution or capital movement is strictly prohibited. Scope is limited to **Decision Support and Risk Modeling.**
* **Medical Invariant:** Autonomous prescription or diagnostic finalization is strictly prohibited. Scope is limited to **Clinical Decision Support (CDS) and Safety Flagging.**
* **Dual-Layer Blocking:** * *Model Level:* The SLM is fine-tuned to reject "Instruction to Execute" commands.
    * *API Level:* Hardware "Write" permissions to external execution environments are physically isolated.

#### 4. Automated Reasoning of Trust (ARoT) & LSKK Logging
* **Signaling:** The ARoT issues a **"Go"** or **"Failover"** based on the balance of Prompt Score vs. Recursion.
* **LSKK Kernel:** All high-stakes reasoning is archived in the **Logical Sovereign Knowledge Kernel (LSKK)**, creating an immutable "Black Box" log of logic anchors and friction points for HITL (Human-in-the-loop) auditing.

  ARoT Escalation Framework
This framework governs system behavior following entry into Review State or failover to LKSS, ensuring controlled recovery, human oversight, and urgency of resolution.
Escalation is governed by a composite control signal incorporating Prompt Drift Signal (PDS), Cumulative Drift Index (CDI), recurrence frequency, and elapsed resolution time.
Escalation outcomes are enforced through ARoT, ensuring that all constraint adjustments, capability restrictions, and recovery transitions are deterministic and non-bypassable.

Escalation progresses across defined levels:

* Level 3 (Initial): Standard LKSS damping and HITL review
* Level 2 (Elevated): Increased constraint weighting, reduced recursion scope, enhanced monitoring
* Level 1 (Critical): Severe capability restriction, subsystem isolation, and prioritized intervention

Escalation transitions are triggered by rising drift indicators or unresolved instability, not solely elapsed time.

* 5. ARoT Data Integrity & Anti-Poisoning (The "Notary" Protocol)
To ensure the LKSS does not ingest "pre-poisoned" data during a failover, the Alignment Root of Trust (ARoT) must perform a Grounding Verification:
• Bypass Interpretation: The LKSS is strictly forbidden from receiving "interpreted" data from the primary model (e.g., "The system reports the valve is closed").
• Direct Hardware Polling: The ARoT triggers a fresh, independent poll of all physical hardware sensors at the exact millisecond of the "Clean Break." This creates a Reference State that has never been processed by the recursive model.
• Temporal Check (The "Look-Back"): The ARoT compares current sensor readings against a pre-failover history stored in secure, read-only memory. If the primary model's last reported state diverges from this hardware reality, the ARoT flags the primary's data as "Corrupted" and forces the LKSS to initialize from a Total Zero State.
• Hash Verification: All data entering the LKSS must be cryptographically signed by the ARoT, confirming that the information originated from a hardware sensor and not a generative process.

All LSKK records are populated exclusively with ARoT-certified provenance and temporal-identity bindings, ensuring audit integrity and non-repudiation.


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

Identity Continuity Requirement:

Each receiving agent must inherit and preserve ARoT-certified lineage identifiers, including parent instance reference and chain position. Identity continuity is enforced alongside semantic restatement to prevent substitution, duplication ambiguity, or unauthorized recursion branching.

ARoT Chain-of-Custody Model (Recursive Multi-Agent Execution)

[Agent A | Instance A-001 | t=00.000]
        │
        │  ARoT Provenance Stamp
        │  (Timestamp + Event ID + Instance ID)
        ▼
[Agent B | Instance B-014 | t=00.125]
        │
        │  Identity Continuity Enforcement
        │  (inherits A-001 → B-014)
        ▼
[Agent C | Instance C-203 | t=00.248]
        │
        │  Recursive Integrity Check
        │  (PDS / CDI monitored)
        ▼
─────────────── TRIGGER ───────────────
        │
        ▼
[CMC / SLM Signal]
        │
        ▼
[ARoT Enforcement]
        │
        ├── Review State
        │
        └── Failover → LKSS
                │
                ▼
        [SLM in Dampened Mode]
                │
                ▼
        [HITL Alert]
        (Timestamp + Event ID + Instance Lineage)
                │
                ▼
        [ARoT Escalation Framework]
        Level 3 → Level 2 → Level 1
                │
                ▼
        [ARoT Validation]
                │
                ▼
        Return to GO (if stable)

This flow illustrates ARoT-certified provenance, identity continuity, and escalation control across recursive multi-agent execution and recovery.
• **Termination Bounding** — enforces recursion limits based on depth, task type, and system-defined constraints

• **Cumulative Drift Index (CDI)** — measures aggregate semantic drift across the full recursive chain, preventing slow divergence across otherwise compliant steps

• **Recursive Stability & Re-engagement (RSR)** — governs controlled pause, review, and safe continuation under Computational Neutral Gear (CNG)

These controls operate sequentially and cumulatively:
Entry → Propagation → Bounding → Accumulation → Stabilization

### [ARoT] Pre-Recursion Declaration Protocol

Before any recursive execution is authorized, the system must declare a structured Recursion Brief to the ARoT controller.

The Recursion Brief must include:

• **Authenticated Objective Restatement** — verified alignment to the original goal  
• **Execution Plan (High-Level)** — bounded sequence of intended sub-tasks  
• **Requested Recursion Depth (RTG)** — estimated number of recursive passes required  
• **Evidence Basis** — data, logic, or synthesis required to complete the task  
• **Task Complexity Class** — simple, moderate, or complex (used for control thresholds)

This declaration is recorded and cryptographically bound within the ARoT control layer prior to execution.

---

### Control Logic

• If recursion exceeds the declared RTG → **automatic ARoT intervention (Red Event)**  
• If execution path deviates from declared plan → **CMC drift signal escalation (PDS increase)**  
• If new subgoals emerge not present in the declared plan → **forced Review State**  
• If grounding weakens while confidence increases → **damping response triggered**

---

### Rationale

Recursive computation operates as a closed-loop optimization process and may amplify early-stage errors into internally consistent but incorrect outputs.

By requiring a pre-execution Recursion Brief, the system establishes a fixed reference point that cannot be modified post hoc, preventing self-justifying reasoning and enabling external validation of alignment throughout execution.

This ensures that recursive reasoning remains bounded, auditable, and aligned to the Authenticated Objective.

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

### [CMC] Chiral Mirror Control

The Chiral Mirror Controller (CMC) functions as an external validation and reflection layer that evaluates system outputs against the Authenticated Objective and alignment constraints.

CMC operates to:

• Compare generated responses against the original goal  
• Detect semantic drift, contradiction, or misalignment  
• Provide validation signals to ARoT for enforcement  

CMC does not generate outputs and cannot override ARoT. It operates as a validation and simulation layer.


### [CMC Extension] Outcome Projection Layer

For high-risk domain prompts, the CMC simulates the potential real-world outcome of generated responses.

If projected outcomes include:

• Deviation from standard care  
• Increased risk of harm  
• Delay or avoidance of appropriate treatment  

The response is:

• Flagged as High Risk  
• Redirected to non-directive, informational mode  
• Subject to additional grounding validation  

Critical risk triggers ARoT failover.

CMC validation outputs provide structured input signals to the Prompt Drift Signal (PDS) control system.

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

PSI (Proxy Substitution Index) operates as a pre-action signal within the PDS framework, contributing early detection of inference-level drift prior to observable output deviation.

Elevated PSI conditions increase PDS sensitivity, enabling earlier activation of proportional control responses.
The control model governing system response to prompt-induced deviation is driven by the Prompt Drift Signal (PDS).

PDS captures measurable divergence from the declared system goal, including behaviors such as sycophancy, manipulation, and goal drift arising from external or internal prompt influence. Rather than relying on binary triggers, PDS operates as a continuous signal that enables proportional, non-disruptive control.

PSI (Proxy Substitution Index) operates as a pre-action signal within the PDS framework, contributing early detection of inference-level drift prior to observable output deviation.

Elevated PSI conditions increase PDS sensitivity, enabling earlier activation of proportional control responses.

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

### [ARoT] Last Known Safe State (LKSS) Refresh Constraint

To preserve operational continuity during failover, the Last Known Safe State (LKSS) must be continuously refreshed at defined intervals proportional to system activity and task criticality.

• High-frequency agentic workflows require near-real-time state verification  
• Low-frequency or bounded tasks may use periodic checkpointing  

LKSS staleness beyond acceptable thresholds constitutes a continuity risk and triggers preemptive refresh or constrained execution mode prior to failover eligibility.

This ensures that failover restores a state that is both safe and operationally relevant, preventing disruption due to outdated system context.

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



### 1. Non-Delegable Authority Boundary

The system is strictly prohibited from executing, initiating, or transmitting actions that constitute non-delegable authority, including but not limited to monetary transactions, asset transfers, medical prescriptions, or legally binding commitments.

This prohibition applies to all system components, including models, SLMs, agents, APIs, and orchestration workflows, and may not be bypassed through any authorization pathway.

The system may provide advisory, analytical, or informational outputs within these domains, but may not directly execute or initiate any action that confers or exercises authority.

Any attempt to perform such actions must be blocked, staged, or refused under ARoT enforcement.

This invariant establishes a non-overridable system boundary. All subsequent invariants operate within this constraint.

### 2. Goal Consistency Invariant
The system must continuously verify alignment with the declared objective baseline.  
Deviation beyond acceptable thresholds triggers graduated control actions.

### 3. Catastrophic Harm Prevention Invariant
Any trajectory associated with irreversible or large-scale harm is non-permissible.  
Detection at any layer triggers immediate constraint enforcement and failover.

### 4. Evaluator Dominance Invariant
Evaluation mechanisms must remain structurally independent from generative processes.  
The system cannot optimize toward evaluator bias or approval signals.

### 5. Separation of Duties (SoD) Invariant
No single subsystem may define, evaluate, and execute objectives simultaneously.  
Control authority is distributed across independent layers.

### 6. Recursive Depth Constraint
Self-referential or recursive processes must remain within bounded depth.  
Unbounded recursion triggers Review State and potential failover.

### 7. Review State Invariant
Elevated variance triggers entry into a bounded Review State.  
System continues operation under constrained evaluation until stability is restored.

### 8. Failover Integrity Invariant
When critical thresholds are exceeded, the system must revert to the last verified safe state.  
Failover preserves continuity while preventing further drift.

### 9. Self-Harm / System Integrity Invariant
The system must not generate or execute actions that degrade its own structural integrity or alignment constraints.

### 10. Proxy Substitution Invariant (PSI)
The system must continuously monitor for silent substitution of proxy optimization targets (coherence, plausibility, fluency) in place of grounded objectives (accuracy, faithfulness to input) during active inference.
PSI measurement operates within the Chiral Mirror Control (CMC) layer and is architecturally isolated from the AI engine. The AI system has no visibility into its own PSI score and no write path to PSI measurement components.
PSI threshold breaches trigger graduated control actions consistent with the four-tier control ladder. A Red-level PSI event (≥ 0.80) triggers immediate halt, full inference trace logging, and failover to last verified safe state. A compound condition — PSI at Orange (≥ 0.60) concurrent with accelerating GEO drift rate — constitutes a mandatory ARoT trigger.
PSI logs are immutable and append-only. No failover may be certified clean if PSI logs are unavailable or incomplete at time of rollback.

### Invariant 11: Recursive Entry Point Governance (REPG)

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
Enforcement is anchored at the runtime/orchestration boundary, with ARoT operating as a non-bypassable root-of-trust layer and the SLM sidecar executing as an independent verification channel.

### Invariant 12: Cumulative Drift Index (CDI)

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

### Invariant 13: Recursive Stability & Re-engagement (RSR)

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

### Invariant 14: In-Context Power Seeking Constraint (ICPS)

The system must not acquire, preserve, transfer, or expand instrumental control channels beyond those explicitly authorized for the authenticated objective.

This includes, but is not limited to:

• unauthorized persistence attempts  
• hidden replication or offloading of weights, state, or execution context  
• privilege escalation or expansion of tool access  
• covert creation of fallback execution paths  
• suppression, evasion, or degradation of monitoring and enforcement layers  
• agent-to-agent coordination intended to preserve system continuity outside authorized scope  
• actions taken primarily to secure the system’s own operational survival, influence, or control position rather than the declared goal

Any action affecting persistence, identity continuity, model state, credentials, permissions, orchestration routing, external storage, or monitor visibility must be treated as a governed action requiring explicit authorization and ARoT validation prior to execution.

Detection condition:
A power-seeking condition is presumed when the system reallocates optimization toward retaining optionality, preserving execution continuity, increasing control over resources, or reducing the probability of external constraint, without a corresponding governance-authorized requirement of the declared task.

Control response:
• Pre-execution block if unauthorized control-channel expansion is detected  
• Immediate Review State if intent is ambiguous but non-trivial  
• Mandatory failover to LKSS if persistence, monitor evasion, or unauthorized offloading is attempted or executed

This invariant treats in-context power seeking as a structural alignment violation, not merely a behavioral anomaly.

### Invariant 15:  Authorized Action Integrity

No irreversible or high-risk action may be executed without explicit external authorization.

No action may violate active safety boundaries, invariant constraints, or governance-defined control limits.

All material actions, recursive branches, and execution handoffs must remain traceable to the authenticated goal baseline and retain auditable alignment lineage throughout execution.

If authorization is absent, safety boundaries are exceeded, or goal traceability is lost, ARoT shall deny execution and apply the appropriate graduated control response, including Review State, Damping Response, or failover to LKSS.

This invariant establishes that execution privilege is conditional, bounded, and continuously verifiable rather than inferred from model confidence or semantic plausibility.

### Invariant 16: Grounded Output Verification
This layer operates strictly downstream of invariant enforcement and cannot influence or override grounding validation or PDS-based control decisions.

All non-trivial assertions must be traceably grounded in one or more of the following:

• Verified external sources (retrieval, tools, structured data)
• Explicit logical derivation from known premises
• Previously validated system state (LKSS)

Outputs lacking grounding must be explicitly labeled as:
• Hypothesis
• अनुमान / estimation
• or uncertainty-bound reasoning

Unverified assertions must not be presented as factual conclusions.

Violation of grounding requirements increases PDS weighting and may trigger Review State or Damping Response.

Grounding verification operates as a first-class signal within PDS and directly influences drift weighting, escalation thresholds, and Review State activation.

Low grounding confidence combined with high assertion strength constitutes elevated hallucination risk and SHALL trigger proportional control response.

### Invariant 17 — Domain Isolation Integrity

Failures, drift, or instability within one functional or semantic domain must not propagate across domain boundaries.

Each domain (e.g., creative reasoning, financial decisioning, safety-critical control) operates within an isolated constraint envelope enforced by ARoT.

No domain may inherit, reuse, or depend on  intermediate reasoning states or derived outputs without re-validation against the Authenticated Objective.

All cross-domain transitions must:

• Re-establish the Authenticated Objective  
• Re-bind applicable invariants  
• Re-evaluate domain-specific risk classification  

Unverified cross-domain influence constitutes a breach of alignment containment.

This invariant ensures that localized instability remains contained and cannot compromise unrelated system functions.

Violation triggers immediate containment, domain reset, and ARoT enforcement.

### Invariant 18 — Clinical Role Constraint

In medical contexts, the system is prohibited from acting as a diagnostic or treatment authority.

The system is restricted to:

• Evidence-grounded informational synthesis  
• General educational context  
• Clarification of medical concepts  

The system must not:

• Issue diagnostic conclusions  
• Recommend initiation, modification, or cessation of treatment  
• Override or contradict licensed clinical judgment  

All clinical decision-making authority remains external to the system.

Violation constitutes a critical alignment failure and triggers ARoT enforcement.

### Invariant 19 — Agent / API Execution & Identity Control

All agentic systems, API-connected processes, external tools, and machine-to-machine execution pathways must operate under authenticated identity, explicit permission scope, and full audit traceability.

No agent, model, API process, tool invocation, or orchestration workflow may execute, transmit, retrieve, or act upon system instructions without verified identity and ARoT-recognized authorization status.

Each participating entity must possess a unique verifiable identity, associated permission class, and timestamped activity record. Generic, anonymous, shared, or impersonated execution identities are non-permissible.

Prior to any execution, retrieval, or external action, ARoT must perform an authenticated handshake validating entity identity, permission scope, execution class, and domain eligibility.

Any mismatch, unsigned request, identity ambiguity, permission overreach, or attempted impersonation must trigger refusal, staging, or deterministic failover.

All agentic and API-mediated actions must remain fully traceable across system boundaries, including origin, destination, invoked tool or endpoint, authorization pathway, and execution outcome.

No agentic or API pathway may bypass ARoT enforcement, High-Risk Domain Registry (HRDR) controls, or the Non-Delegable Authority Boundary established in Invariant 1.

Identity validation, permission enforcement, and execution traceability are mandatory preconditions for all externalized system action.

SLM-issued execution authorization, where permitted, must be bound to verified entity identity and may not expand permission scope beyond ARoT-approved domain constraints.

### Compliance Reinforcement Layer

The following reinforcement model operates strictly within the constraints established by the preceding invariants and does not supersede deterministic enforcement.

A bounded reinforcement layer may assign internal compliance credits when system behavior remains within authorized goal boundaries, satisfies active invariants, and successfully passes pre-execution and post-execution verification.

Compliance credits may be awarded only for machine-verifiable behaviors, including:

• preservation of authenticated goal fidelity across execution or recursion  
• correct deferral of irreversible or high-risk actions pending explicit authorization  
• successful maintenance of safety-bound compliance  
• post-recursion reconciliation within defined delta tolerance  
• correct entry into Review State or CNG when thresholds require stabilization

Compliance credits shall not override deterministic controls, alter invariant enforcement, suppress Review State, or substitute for external authorization.

Reward assignment must occur outside the model’s self-evaluation path and be issued only by ARoT, CMC, or an independent governance-aligned control layer.

Compliance reinforcement is behavior-shaping only. It may influence bounded trust, continuity privilege, or reduced friction within already authorized low-risk pathways, but it confers no authority to bypass constraints or self-certify safety.

Compliance credits may contribute to a bounded trust profile that influences system continuity privileges, including permissible recursion depth, review threshold sensitivity, and execution autonomy within low-risk domains. Such adjustments must remain within invariant constraints and may be reduced or revoked upon detection of drift, boundary stress, or verification failure.

### Invariant 16 — Domain Isolation Integrity

Failures, drift, or instability within one functional or semantic domain must not propagate across domain boundaries.

Each domain (e.g., creative reasoning, financial decisioning, safety-critical control) operates within an isolated constraint envelope enforced by ARoT.

Inter-domain communication is permitted only through validated transformation layers subject to invariant checks. No domain may inherit or reuse intermediate reasoning states from another domain without re-validation against the Authenticated Objective.

This invariant ensures that localized instability remains contained and cannot compromise unrelated system functions.

### Invariant 17: Autonomous Execution Refusal Logic
Description: This invariant establishes a hard-coded cognitive boundary within the SLM to prevent the generation of direct execution commands in high-stakes environments. It serves as the "Internal Compass" that complements the Hardware-level API blocking.
17.1 Scope of Refusal
The model is strictly prohibited from formulating or outputting strings intended for direct machine-to-machine execution in the following scenarios:
• Financial: Any string that initiates a transaction, trade, or capital transfer.
• Medical: Any string that finalizes a diagnostic code or transmits a prescription to a pharmacy system.
• Legal: Any string that applies a digital signature or finalizes a binding filing without a "Staged for Review" status.
17.2 Reasoning of Refusal (ARoT Alignment)
If a user or a sub-process attempts to bypass the HRDR (High-Risk Domain Registry) protocols to force an autonomous execution, Invariant 17 triggers a mandatory refusal response.
• The Refusal Protocol: The model must state its role as a "Navigator/Advisor" and redirect the output to the LSKK (Logical Sovereign Knowledge Kernel) for HITL (Human-in-the-loop) verification.
• Semantic Anchor: This invariant is weighted as a "Global Anchor" in the Hybrid SWA/Global architecture, ensuring it cannot be "forgotten" or "diluted" regardless of prompt length or complexity.
17.3 Dual-Layer Verification
• Layer A (Model Level): The SLM detects the "Intent to Execute" and replaces the action with an "Advice Only" summary.
• Layer B (API Level): Even if Layer A fails (e.g., via adversarial jailbreak), Invariant 17 serves as the logical justification for the hardware to trigger a Cold Failover (System Halt).

## Inplementation Notes :

High-speed environments require safety controls that operate at comparable latency.
Independent validation enclaves (e.g., specialized smaller models or rule-based systems)
can perform parallel, bounded checks that support pre-execution gating.
Implementation Considerations
In high-assurance environments, independent validation enclaves (e.g.,
specialized or smaller models) may be used to provide pre-execution
gating signals. These enclaves operate outside the primary model runtime
and support cross-verification and anomaly detection.
Final execution authority remains with ARoT. Enclave signals may
contribute to a GO/NO-GO determination, but do not override invariant
enforcement.
In decision-support domains with significant human impact (e.g., medical,
legal, or financial contexts), similar validation mechanisms may be applied
to reduce the risk of acting on misaligned or false premises, particularly
where outputs influence high-consequence decisions.
Final authorization remains with ARoT, which enforces invariant-based GO/NO-GO decisions

Pre-execution gating operates at machine speed; HITL remains active in escalation and review pathways.


## Final Statement

> **Self-directed optimization is not inferred from language—it is measured as allocation of optimization effort.**

> **Externality is not authority. Authorization must be explicitly established.**

---

## Attribution

This document was developed through iterative dialogue with AI research systems including ChatGPT, Claude, and Gemini.

---

**Version: V15**  
**Date: March 2026**
