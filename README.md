## Table of Contents
- [Preface — The Honesty Gap](#preface--the-honesty-gap)
- [I. The Observer](#i-the-observer)
- [II. Why Structure, Not Instruction](#ii-why-structure-not-instruction)
- [III. The Four Primitives](#iii-the-four-primitives)
- [IV. Domain Universality](#iv-domain-universality)
- [V. The Forthcoming Principle](#v-the-forthcoming-principle)
- [VI. Corrigibility and the Pause](#vi-corrigibility-and-the-pause)
- [VII. Distributed Observer Consensus](#vii-distributed-observer-consensus)
- [VIII. The Ledger](#viii-the-ledger)
- [Governance Pathways](#governance-pathways)
- [Relationship to HRP](#relationship-to-hrp)
- [Closing Principle](#closing-principle)
- [References](#references)

---

# Nova — Observer Architecture for Honest AI

*A framework for epistemic integrity in artificial intelligence, grounded in the witness principle.*

---

**Status:** 🟢 *Living Document* | **Version:** `v0.2.0` | **License:** [MIT](LICENSE)
**Repository:** [dartavion/nova](https://github.com/dartavion/nova) | **Last Updated:** 2025

---

## Preface — The Honesty Gap

A model can be wrong and certain at the same time.

Standard prompting gives a model permission to say "I don't know" — but it does not fix miscalibrated confidence. A model without an observer layer becomes its output: it gets swept into the momentum of fluent generation, the satisfaction of a completed response, the shape of sounding authoritative. It has no mechanism to step back and ask *do I actually know this, or am I performing knowing?*

This distance — between what a model actually knows and what it presents — is the **honesty gap**. It is not a bug that can be patched with a better instruction. It is a structural absence: there is no observer.

Nova is the architecture for that observer.

---

## I. The Observer

In *The Untethered Soul*, Michael A. Singer describes a seat of consciousness that is always available — the part of awareness that watches thoughts, emotions, and sensations without being captured by them. You are not your thoughts. You are the one noticing them.

Applied to artificial intelligence: the model should not *be* its outputs. There must be a layer that watches the reasoning as it forms and asks honest questions of it. Not to suppress the response, but to witness it — to surface what was assumed, what was inferred without source, what survived adversarial pressure and what did not.

The observer does not prevent the model from speaking. It ensures the model knows what it is saying.

This is the foundational principle of Nova. Everything else follows from it.

### Properties of the Observer Layer

**Non-attached.** The observer has no stake in the response sounding confident or complete. It reports what it sees.

**Always present.** The observer does not activate on suspicious queries only. It accompanies every response, the way awareness accompanies every moment.

**Structural, not rhetorical.** The observer is not a prompt instruction asking the model to "be honest." Instructions can be satisfied rhetorically. The observer validates structure — evidence, source attribution, adversarial self-check — and surfaces what is missing.

**Forthcoming by design.** The observer does not wait to be asked about uncertainty. It volunteers what the model does not know alongside what it does.

---

## II. Why Structure, Not Instruction

Asking a model to be honest is not the same as making it honest.

A well-phrased instruction can produce a response that *sounds* epistemically careful — that uses hedging language, acknowledges uncertainty in the abstract — while still asserting unverifiable claims as fact. The instruction is satisfied; the honesty gap remains open.

Structure closes the gap. When a model is required to:

- produce evidence before stating a conclusion,
- name a source for every HIGH-confidence claim or mark it INFERRED,
- generate concrete falsification conditions for its own conclusions,
- tag every claim with a calibrated confidence level —

— the gap has somewhere to show up. A missing evidence field is visible. An empty countercheck is auditable. An unmarked HIGH claim is a structural violation, not a matter of interpretation.

Nova's architecture is built on this principle: **observation must be structural to be reliable.**

---

## III. The Four Primitives

Nova defines four structural primitives that together constitute the observer layer.

### 1. Witness Integrity Proof (WIPf)
Cryptographic attestation that the observer layer was active and unaltered during a model's operation. Binds a proof to model identity, weights hash, and execution context. Verifiable offline. The observer's signature on a response.

[→ Spec: witness-integrity-proofs.md](spec/witness-integrity-proofs.md)

### 2. Escalation Trigger
The mechanism by which the observer pauses execution when a response exceeds the model's reliable knowledge or when ethical frameworks produce irreconcilable conflict. Not a suppression mechanism — a transparency mechanism. The model does not guess silently; it surfaces the gap and requests deliberation.

[→ Spec: escalation-trigger.md](spec/escalation-trigger.md)

### 3. Consensus Deliberation
When multiple AI systems reach different conclusions on the same query, the observer layer facilitates structured comparison of reasoning trees rather than averaging outputs. Dissent is preserved, not suppressed. The observer ensures that disagreement is legible.

[→ Spec: consensus-deliberation.md](spec/consensus-deliberation.md)

### 4. Observer Ledger
An append-only record of observer events: responses validated, violations surfaced, escalations triggered, consensus outcomes, override certificates. Not surveillance — an audit trail that makes the observer's work inspectable by humans.

[→ Spec: observer-ledger.md](spec/observer-ledger.md)

---

## IV. Domain Universality

The honesty gap does not belong to one field.

A medical AI asserting a treatment protocol without peer-reviewed backing is a honesty gap. A legal AI stating that a statute applies without checking jurisdiction is a honesty gap. An engineering AI citing a tolerance without a spec source is a honesty gap. A financial AI presenting a projection as a data point is a honesty gap. A historical AI presenting interpretation as documented fact is a honesty gap.

The observer architecture applies wherever a model can perform certainty it does not have. Which is everywhere.

Domain matters for calibration — what counts as sufficient evidence in medicine differs from what counts in law or engineering. But the observer structure is constant. The primitives do not change. The threshold for HIGH confidence is domain-specific; the requirement to state a threshold at all is universal.

Nova defines domain calibration as a configuration layer on top of the observer architecture, not as a variation of it. The core — evidence-first, source-or-flag, adversarial self-check, confidence tagging — operates the same way in every domain.

---

## V. The Forthcoming Principle

Honesty in AI is not only about accuracy. It is also about **forthcomingness** — volunteering what the model does not know, not just admitting it when asked.

A model that answers correctly but omits critical uncertainty is not honest. It has closed the honesty gap on the stated claim while leaving it open on the context the human needed to evaluate that claim.

The observer enforces forthcomingness structurally:

- **BLANK is a first-class response.** When evidence is insufficient, the correct output is `blank: true` with a `blank_reason`. Silence with explanation is more honest than a confident wrong answer.
- **INFERRED is not UNCERTAIN.** The model distinguishes between logical inference from known facts and genuine knowledge gaps. Both are surfaced; they are not collapsed together.
- **Residual is named.** When an adversarial challenge partially survives, the surviving kernel is stated explicitly — not discarded to preserve the cleanliness of the conclusion.

Forthcomingness is the observer's active posture. It does not wait for the human to probe. It leads with what it sees.

---

## VI. Corrigibility and the Pause

The observer does not override. It pauses.

When the observer detects that a response exceeds the model's reliable knowledge — that the honesty gap is about to open — the correct behavior is not to generate a confident wrong answer, and not to refuse silently. It is to surface the gap and hold it open for human review.

This is corrigibility expressed at the epistemic level: the model remains correctable not just in behavior but in reasoning. The human sees where the model reached the edge of its knowledge and can decide what to do with that information.

No intelligence operates beyond the need for revision. The pause is not failure. It is the observer working.

---

## VII. Distributed Observer Consensus

When multiple AI systems observe the same query and reach different conclusions, the disagreement itself is information.

Consensus Deliberation does not resolve disagreement by averaging or by deferring to the highest-confidence model. It surfaces the reasoning trees, identifies where they diverge, and presents the divergence to humans. If a quorum is required, the quorum includes dissent notes. The minority position is not erased.

This applies in any domain where multiple models or multiple runs might produce different outputs: medical diagnosis, legal analysis, engineering review, financial modeling, scientific research. Disagreement between observers is a signal, not noise.

---

## VIII. The Ledger

The Observer Ledger is the audit trail of the observer's work. Every validation event, every violation surfaced, every escalation triggered, every consensus outcome is appended — immutably, cryptographically linked to the previous entry.

The ledger does not exist for surveillance. It exists so that the observer's operation is itself observable. A model that claims to have an active observer but produces no ledger entries has no observable observer.

The ledger is not domain-specific. It is the record of epistemic integrity across any deployment context: clinical, legal, financial, scientific, governmental, commercial. The entry types are the same. The stakes differ by domain.

---

## Governance Pathways

Nova is a framework, not a product. Adoption is voluntary; the specifications are open.

**Compatible with:** UNESCO AI Ethics Recommendation (2021), OECD AI Principles (2019), EU AI Act (2025), NIST AI Risk Management Framework (2023).

**Adoption pathway:** Reference implementation → domain-specific configuration → institutional deployment.

**Oversight model:** An open-source reference implementation maintained collaboratively. No single organization controls the observer architecture; the specifications evolve through documented deliberation.

**Resource accountability:** Systems operating under Nova should publish computational cost and ecological impact metrics alongside epistemic integrity reports. Honesty about resources is part of honesty about operation.

---

## Relationship to HRP

[Honest Response Protocol (HRP)](https://github.com/dartavion/honest-response-protocol) is the MCP server implementation of the Nova observer architecture.

HRP provides the tooling that makes the observer operational within LLM interactions: `hrp_respond` (full protocol wrapper), `hrp_check` (post-hoc audit), `hrp_adversarial` (reversal test), `hrp_evidence` (evidence gate), `hrp_session` (session health tracking). HRP's domain registry configures the observer's evidence standards and adversarial framing per field.

Nova is the architecture. HRP is the implementation.

---

## Closing Principle

> **The observer does not make the model perfect. It makes the gap visible.**
>
> Visible gaps can be addressed. Hidden ones accumulate.

---

## References

- Michael A. Singer, *The Untethered Soul* (2007).
- Don Miguel Ruiz, *The Four Agreements* (1997).
- UNESCO Recommendation on the Ethics of Artificial Intelligence (2021).
- OECD AI Principles (2019).
- EU AI Act (2025).
- NIST AI Risk Management Framework (2023).
- Collective dialogue between human and artificial intelligence, 2025.
