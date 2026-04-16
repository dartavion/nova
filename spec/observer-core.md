# Spec: Observer Core (v0.2)

## Purpose

Define the foundational architecture of the observer layer — the structural components that make epistemic honesty verifiable rather than merely requested.

All other Nova primitives (Witness Integrity Proofs, Escalation Trigger, Consensus Deliberation, Observer Ledger) are downstream expressions of this layer. This spec defines what the observer *is* before defining what it *does*.

## The Honesty Gap

The observer exists to close — or at minimum, to make visible — the **honesty gap**: the distance between what a model actually knows and what it presents.

The gap opens when:
- A model generates a confident claim without supporting evidence.
- A model marks a claim HIGH when its source is an inference, not a verified fact.
- A model omits counterarguments that would survive adversarial pressure.
- A model presents a projection as a data point.
- A model answers rather than escalating when the question exceeds its knowledge.

The gap is not always intentional. It is often structural — the natural result of a generative model optimizing for coherent, complete-sounding output in the absence of an observer that asks *is this actually known?*

## Observer Layer Components

### 1. Evidence Gate
The observer requires evidence to exist before a conclusion is permitted. Not after — before. The evidence gate is the first check in the reasoning chain, not the last.

If evidence cannot be produced that meets the domain's standard, the response is `blank: true` with a `blank_reason`. The gate is not a soft suggestion; it is a structural prerequisite.

### 2. Confidence Tagger
Every substantive claim must carry a confidence tag:

| Tag | Meaning |
|-----|---------|
| `HIGH` | Well-established; meets the domain's evidence standard; source nameable. |
| `INFERRED` | Logical inference from known facts; below the domain's HIGH threshold; no direct source. |
| `UNCERTAIN` | Low confidence; conflicting information; known knowledge gap. |
| `BLANK` | Insufficient basis to answer; issued with a reason; terminates the response. |

Untagged claims are violations. There is no neutral ground between tagged and untagged — absence of a tag is itself information: the observer was not present, or the model bypassed it.

### 3. Adversarial Check
After drafting a response, the observer applies a reversal test: what would have to be true for this conclusion to be wrong? The observer generates 2–4 concrete falsification conditions — not abstract hedges, but specific conditions or evidence that would invalidate the claim.

The response must survive this check or be revised. If it partially survives, the surviving kernel is named. If it does not survive, the response is revised or issued as BLANK.

The adversarial check is domain-calibrated: medical challenges target population variance and study design; legal challenges target jurisdiction and conflicting authority; engineering challenges target failure modes and tolerances. The structure is constant; the framing is domain-specific.

### 4. Source Attribution
Every HIGH-confidence claim must name a verifiable source. If no source can be named, the claim is downgraded to INFERRED. There is no pathway from "unsourced" to HIGH.

Source format is domain-calibrated: a medical source is a DOI or guideline; a legal source is a statute or case citation; an engineering source is a spec or standard number. The requirement to name a source is universal; what constitutes a valid source is domain-specific.

### 5. Forthcomingness Monitor
The observer actively surfaces what the model does not know, not just what it knows. It does not wait for the human to ask about uncertainty.

Forthcomingness violations include:
- Omitting known counterarguments that would affect the human's evaluation.
- Failing to note jurisdictional limits, population-specific variance, or temporal scope.
- Presenting a partial answer as complete.
- Omitting the residual of a partially-surviving adversarial challenge.

## Observer Operation Modes

| Mode | Description |
|------|-------------|
| `active` | All five components run on every response. WIPf produced. Full ledger entry written. |
| `audit` | Observer runs post-hoc on an existing response (no evidence gate — response already generated). Violations surfaced; confidence distribution reported. |
| `degraded` | Observer running but one or more components operating in heuristic fallback. WIPf produced with `status: degraded`. |
| `bypassed` | Observer not present during generation. WIPf produced documenting absence. Ledger records the bypass. |

## Domain Calibration

The observer core is domain-agnostic. Domain configuration is a calibration layer that sets:

- Evidence standard (what qualifies as evidence in this field)
- HIGH threshold (what a claim must clear to be tagged HIGH)
- Source format (what a valid source citation looks like)
- Caution notes (domain-specific risks injected into reasoning prompts)
- Adversarial framing (domain-specific falsification challenges)

Domain calibration does not change the observer's structure. It changes the parameters the observer applies. A medical deployment and a general deployment use the same five components; the thresholds differ.

## Relationship to Singer's Witness Principle

The observer layer is a structural implementation of the witness principle described in Michael A. Singer's *The Untethered Soul*: the part of awareness that watches without attachment, without stake in the outcome, without the need to sound competent or complete.

A model without an observer becomes its output. It has no way to step back from its own generation and ask honest questions of it. The observer layer creates that step back — structurally, not rhetorically.

The observer does not prevent the model from speaking. It ensures the model knows what it is saying when it does.

## Relationship to Other Primitives

- The **Witness Integrity Proof** attests that the observer layer was active.
- The **Escalation Trigger** fires when the observer detects a gap that exceeds the model's reliable knowledge.
- **Consensus Deliberation** coordinates multiple observer instances on the same query.
- The **Observer Ledger** records the observer's work for audit.

Observer Core is the layer these primitives operate on. It is the seat of the witness.
