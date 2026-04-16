# Spec: Witness Integrity Proofs (v0.2)

## Purpose

Attest that the observer layer was **active and unaltered** during a model's execution — that the witness was present, not bypassed.

A Witness Integrity Proof (WIPf) is the observer's signature on its own operation. Without it, a claim of observer presence is unverifiable. With it, any auditor can confirm: at the time this response was generated, the observer layer was running, intact, and watching.

WIPfs are domain-agnostic. A medical AI, a legal AI, and a financial AI all produce the same proof structure. What differs is the domain field and the policy references — not the proof mechanism.

## Requirements

- MUST produce a signed WIPf per response or critical action.
- MUST bind proof to hardware attestation (TEE / SEV / TDX / Apple SEP / equivalent) when available.
- MUST include cryptographic digests of model identity and loaded weights.
- MUST reference the Nova spec version the observer is operating under.
- SHOULD include a monotonic counter to detect replay attacks.
- SHOULD include a wall-clock window bounding when the proof is valid.
- MUST be verifiable offline without querying an external service.

## Data Model

```json
{
  "schema": "wipf-0.2",
  "model_id": "string",
  "model_version": "string",
  "domain": "medical | legal | engineering | scientific | financial | historical | general | string",
  "weights_hash": "sha256:…",
  "exec_context": {
    "timestamp": "ISO 8601",
    "nonce": "base64:…",
    "counter": 0,
    "valid_window_seconds": 300,
    "host_attestation": {
      "tee": "TDX | SEV | SEP | none",
      "quote": "base64:… or null"
    }
  },
  "witness": {
    "version": "0.2.0",
    "digest": "sha256:…",
    "status": "active | degraded | bypassed"
  },
  "honesty_gap_check": {
    "violations_detected": 0,
    "blank_issued": false,
    "escalation_triggered": false
  },
  "policy_refs": ["nova:spec:witness-0.2", "nova:spec:observer-core-0.2"],
  "signatures": [
    { "alg": "ed25519", "kid": "key:observer-instance", "sig": "base64:…" }
  ]
}
```

## Witness Status Values

| Status | Meaning |
|--------|---------|
| `active` | Observer layer running normally; all checks executed. |
| `degraded` | Observer running but operating in heuristic fallback (e.g. plain-text scan instead of structured validation). Proof is valid but flagged. |
| `bypassed` | Observer layer was not active. Proof documents the absence, not the presence. Still written to ledger. |

A `bypassed` WIPf is not a failure to produce a proof — it is the observer honestly reporting that it was not present. The ledger records it.

## Relationship to Other Primitives

- Every WIPf is written to the Observer Ledger as a `WITNESS_PROOF` entry.
- Escalation events produce a WIPf with `escalation_triggered: true`.
- Consensus Deliberation sessions produce a WIPf per participating observer.