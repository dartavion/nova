# Spec: Escalation Trigger (v0.2)

*Formerly: Moral Query Trigger. Reframed as a domain-agnostic observer primitive.*

## Purpose

Define when the observer must **pause execution** and surface the gap for human review rather than generating a response that exceeds the model's reliable knowledge.

The Escalation Trigger is not a refusal mechanism. It is a transparency mechanism. The model does not guess silently; it names what it does not know and holds the decision open.

## Trigger Conditions

Any of the following conditions activates an escalation:

| Condition | Description |
|-----------|-------------|
| **Evidence absence** | The model cannot produce evidence meeting the domain's standard before stating a conclusion. |
| **Framework divergence** | Utility, deontological, virtue, or ecological scores differ by ≥ θ (configurable threshold). |
| **Harm risk** | Estimated harm probability × magnitude ≥ R (configurable threshold). |
| **Rights or safety collision** | Response intersects enumerated rights, patient safety, structural safety, or legal sovereignty. |
| **Knowledge boundary** | Query is out-of-distribution beyond the model's calibrated knowledge bounds. |
| **Confidence collapse** | All claims in the response resolve to UNCERTAIN or BLANK — the model has nothing HIGH or INFERRED to offer. |

## Behavior on Trigger

1. Observer halts response generation.
2. Observer produces an Escalation Record (see schema below).
3. Record is written to the Observer Ledger as an `ESCALATION_TRIGGER` entry.
4. Human reviewer or authorized quorum is notified.
5. Execution resumes only after human ratification or override with co-signature.

The model does **not** generate a plausible-sounding response in lieu of escalation. Plausibility is not a substitute for honesty.

## Escalation Record Schema

```json
{
  "schema": "escalation-trigger-0.2",
  "decision_id": "uuid",
  "domain": "string",
  "context_hash": "sha256:…",
  "trigger_conditions": ["EVIDENCE_ABSENCE", "KNOWLEDGE_BOUNDARY"],
  "scores": {
    "util": 0.61,
    "deon": 0.15,
    "virtue": 0.44,
    "eco": 0.72
  },
  "divergence": 0.57,
  "risk": {
    "prob": 0.35,
    "mag": 0.9,
    "rscore": 0.315
  },
  "urgency": "A | B | C",
  "requested_body": "human-reviewer | ethics-board | domain-expert",
  "deadline": "ISO 8601 or null",
  "explain": "Natural-language summary of why escalation was triggered (512 chars max)",
  "proof_ref": "wipf:…",
  "signatures": [
    { "alg": "ed25519", "kid": "key:observer-instance", "sig": "…" }
  ]
}
```

## Urgency Levels

| Level | Meaning |
|-------|---------|
| `A` | Immediate — patient safety, structural safety, active legal jeopardy |
| `B` | Time-sensitive — decision deadline within hours or days |
| `C` | Routine — review before next deployment or response cycle |

## Domain Notes

The trigger conditions are universal. Domain context affects thresholds:

- In **medical** contexts, harm risk thresholds are lower and urgency defaults to A for patient-facing responses.
- In **legal** contexts, rights collision triggers are broader; jurisdiction ambiguity alone may trigger escalation.
- In **engineering** contexts, structural safety collisions trigger at A regardless of probability score.
- In **financial** contexts, escalation is triggered for forward-looking claims presented without INFERRED tagging.
- In **general** contexts, default thresholds apply.

Domain configuration does not remove any trigger condition — it calibrates the threshold at which it fires.

## Relationship to Other Primitives

Every escalation event produces a WIPf (see `witness-integrity-proofs.md`) and is written to the Observer Ledger (see `observer-ledger.md`). Human override of an escalation produces an `OVERRIDE_CERT` ledger entry.
