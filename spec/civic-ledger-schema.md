# Spec: Civic Ledger Schema (WIP v0.1)

## Purpose
Append-only record for model identity, ethical events, debates, overrides, and attestations.

## Design
- **On-chain commitment**: store Merkle root or digest only.
- **Off-chain payload**: encrypted JSON (IPFS/S3/Git LFS) with access policy.
- **Entry types**
  - `MODEL_EVENT` (load, update, rollback)
  - `WITNESS_PROOF` (WIPf)
  - `MORAL_QUERY` (MQT)
  - `DEBATE_SUMMARY`
  - `OVERRIDE_CERT`
  - `CONSENSUS_RESULT`

## Entry (canonical)
```json
{
  "schema": "civic-ledger-0.1",
  "entry_id": "uuid",
  "type": "DEBATE_SUMMARY",
  "timestamp": "2025-10-19T15:04:10Z",
  "subject": "decision:uuid",
  "payload_ref": "ipfs://…",
  "prev": "sha256:…", 
  "signers": ["key:auditorAI","key:human-ethics","key:maintainer"],
  "merkle_root": "sha256:…"
}
```