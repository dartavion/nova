# Spec: Moral Query Trigger (WIP v0.1)

## Purpose
Define when a system must **pause** and request human deliberation.

## Trigger Conditions (any → escalate)
- **Framework divergence**: Utility vs Deontology vs Virtue vs Ecological scores differ by ≥ θ.
- **Harm risk**: Estimated harm probability × magnitude ≥ R.
- **Rights collision**: Identified conflict with enumerated rights list.
- **Novelty**: Out-of-distribution context beyond calibrated bounds.

## Data Model (JSON)
```json
{
  "schema": "mqt-0.1",
  "decision_id": "uuid",
  "context_hash": "sha256:…",
  "scores": { "util": 0.61, "deon": 0.15, "virtue": 0.44, "eco": 0.72 },
  "divergence": 0.57,
  "risk": { "prob": 0.35, "mag": 0.9, "rscore": 0.315 },
  "urgency": "B", 
  "requested_body": "EthicsBoard:v1",
  "deadline": "2025-10-21T00:00:00Z",
  "explain": "Natural-language summary (512 chars)",
  "proof_ref": "wipf:…",
  "signatures": [{ "alg":"ed25519", "kid":"key:model", "sig":"…" }]
}
```