# Spec: Witness Integrity Proofs (WIP v0.1)

## Purpose
Attest that a model executed with **non-bypassable oversight** and that the witness layer was active and unaltered.

## Requirements (MUST/SHOULD)
- MUST produce a signed **Witness Integrity Proof (WIPf)** per critical action.
- MUST bind proof to a **hardware attestation** (TEE/SEV/TDX/Apple SEP/etc.) when available.
- MUST include cryptographic digests of model identity and loaded weights.
- SHOULD include a monotonic counter and wall-clock window.
- SHOULD be verifiable offline.

## Data Model (JSON)
```json
{
  "schema": "wipf-0.1",
  "model_id": "nova:demo:classifier",
  "model_version": "2025-10-19+abc123",
  "weights_hash": "sha256:…",
  "exec_context": {
    "timestamp": "2025-10-19T14:32:05Z",
    "nonce": "base64:…",
    "host_attestation": { "tee": "TDX", "quote": "base64:…" }
  },
  "witness": {
    "version": "1.0.0",
    "digest": "sha256:…",
    "status": "active"
  },
  "policy_refs": ["manifesto:VIII", "spec:witness-0.1"],
  "signatures": [
    { "alg": "ed25519", "kid": "key:maintainer", "sig": "base64:…" }
  ]
}
```