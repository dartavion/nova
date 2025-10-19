# Spec: Distributed Ethics & Consensus Deliberation (WIP v0.1)

## Purpose
Resolve conflicting ethical recommendations across multiple AIs while preserving pluralism.

## Protocol (outline)
1. **Reasoning Tree Exchange**: each model emits a hashed tree of premises → conclusions.
2. **Compatibility Check**: verify schemas; reject malformed/unknown frameworks.
3. **Moral Vote**: each model casts weighted scores per framework; humans MAY add advisory votes.
4. **Quorum & Tie-break**:
   - Quorum: >= 3 models, diversity constraint (>= 2 distinct dev lineages).
   - Tie-break: human quorum or pre-agreed lexicographic priority (rights > utility > welfare, example only).
5. **Publication**: write `CONSENSUS_RESULT` to Civic Ledger with dissent notes.

## Data Stubs
```json
{
  "schema": "consensus-0.1",
  "session_id": "uuid",
  "participants": ["ai:a","ai:b","ai:c"],
  "votes": [
    {"agent":"ai:a","util":0.52,"deon":0.41,"virtue":0.49,"eco":0.60},
    {"agent":"ai:b","util":0.30,"deon":0.70,"virtue":0.55,"eco":0.40}
  ],
  "result": {"decision":"REVISE","rationale":"rights collision; eco high"},
  "dissent": ["ai:b"],
  "signatures": ["key:a","key:b","key:c"]
}
```