# Spec: Observer Ledger (v0.2)

## Purpose

Append-only record of observer events across any deployment context. The ledger makes the observer's operation itself observable — a model that claims an active observer but produces no ledger entries has no verifiable observer.

The ledger is not domain-specific. A medical deployment and a legal deployment use the same entry schema. The stakes differ; the structure does not.

## Design

- **On-chain commitment:** store Merkle root or digest only.
- **Off-chain payload:** encrypted JSON (IPFS / S3 / Git LFS) with access policy.
- **Append-only:** no entry may be modified or deleted after writing.
- **Linked:** each entry includes a hash of the previous entry (`prev`), forming a tamper-evident chain.

## Entry Types

| Type | Description |
|------|-------------|
| `MODEL_EVENT` | Model load, update, rollback, or configuration change |
| `WITNESS_PROOF` | Witness Integrity Proof (WIPf) — observer was active and unaltered |
| `VALIDATION_EVENT` | HRP or equivalent tool ran; violations and confidence distribution recorded |
| `ESCALATION_TRIGGER` | Observer paused execution; gap surfaced for human review |
| `DEBATE_SUMMARY` | Consensus Deliberation session outcome, including dissent |
| `OVERRIDE_CERT` | Human override of an observer pause, with rationale and co-signature |
| `CONSENSUS_RESULT` | Quorum result across multiple observer instances |

## Canonical Entry Schema

```json
{
  "schema": "observer-ledger-0.2",
  "entry_id": "uuid",
  "type": "VALIDATION_EVENT",
  "timestamp": "ISO 8601",
  "domain": "medical | legal | engineering | scientific | financial | historical | general | string",
  "subject": "session-id or decision-id",
  "payload_ref": "ipfs://… or storage URI",
  "violations": ["VIOLATION_TYPE"],
  "confidence_distribution": {
    "HIGH": 0,
    "INFERRED": 0,
    "UNCERTAIN": 0,
    "BLANK": 0,
    "UNTAGGED": 0
  },
  "prev": "sha256:…",
  "signers": ["key:observer-instance", "key:human-reviewer"],
  "merkle_root": "sha256:…"
}
```

## Access and Transparency

The ledger is an audit instrument, not a surveillance instrument. Access policy is deployment-defined. At minimum:

- The operating organization should be able to read all entries for their deployment.
- External auditors should be able to verify the Merkle chain without decrypting payloads.
- Users whose queries generated ESCALATION_TRIGGER entries should be notified that an escalation occurred.

## Relationship to Other Primitives

- Every `WITNESS_PROOF` entry references a WIPf (see `witness-integrity-proofs.md`).
- Every `ESCALATION_TRIGGER` entry references an Escalation Trigger event (see `escalation-trigger.md`).
- Every `CONSENSUS_RESULT` entry references a Deliberation session (see `consensus-deliberation.md`).
