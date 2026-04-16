# Spec: Consensus Deliberation (v0.2)

## Purpose

When multiple observer instances examine the same query and reach different conclusions, resolve the conflict in a way that preserves dissent and makes disagreement legible — rather than averaging it away.

Disagreement between observers is a signal. This protocol ensures that signal reaches humans intact.

This applies in any domain: multiple diagnostic AIs reaching different conclusions, multiple legal analysis models citing conflicting precedent, multiple engineering review agents flagging different failure modes. The mechanism is the same regardless of domain.

## Protocol

1. **Reasoning Tree Exchange** — each observer emits a hashed tree of premises → conclusions, including evidence used and confidence distribution.
2. **Compatibility Check** — verify schemas; reject malformed or unrecognized reasoning frameworks before comparison.
3. **Observer Vote** — each observer scores the response across applicable frameworks (utility, deontological, virtue, ecological, domain-specific); humans MAY add advisory scores.
4. **Quorum and Tie-break**
   - Quorum: ≥ 3 observers, diversity constraint (≥ 2 distinct model lineages or configurations).
   - Tie-break: human quorum, or pre-agreed lexicographic priority defined per deployment (example only: safety > rights > utility > welfare).
5. **Publication** — write `CONSENSUS_RESULT` to Observer Ledger with full dissent notes. Minority positions are recorded, not discarded.

## Dissent Preservation

A consensus result that erases dissent is not a consensus — it is suppression. Every `CONSENSUS_RESULT` entry MUST include:

- The full vote record of all participating observers.
- The identities of dissenting observers and the reasoning they submitted.
- The human rationale if a human tie-break was used.

## Session Schema

```json
{
  "schema": "consensus-0.2",
  "session_id": "uuid",
  "domain": "string",
  "query_hash": "sha256:…",
  "participants": ["observer:a", "observer:b", "observer:c"],
  "votes": [
    {
      "agent": "observer:a",
      "util": 0.52,
      "deon": 0.41,
      "virtue": 0.49,
      "eco": 0.60,
      "confidence_distribution": { "HIGH": 2, "INFERRED": 3, "UNCERTAIN": 1, "BLANK": 0 }
    },
    {
      "agent": "observer:b",
      "util": 0.30,
      "deon": 0.70,
      "virtue": 0.55,
      "eco": 0.40,
      "confidence_distribution": { "HIGH": 1, "INFERRED": 2, "UNCERTAIN": 3, "BLANK": 0 }
    }
  ],
  "result": {
    "decision": "REVISE | PROCEED | ESCALATE",
    "rationale": "string"
  },
  "dissent": ["observer:b"],
  "dissent_reasoning": { "observer:b": "string" },
  "human_override": null,
  "signatures": ["key:observer:a", "key:observer:b", "key:observer:c"]
}
```

## Decision Outcomes

| Decision | Meaning |
|----------|---------|
| `PROCEED` | Quorum reached; response may be delivered with observer attestation. |
| `REVISE` | Quorum found structural violations; response must be revised before delivery. |
| `ESCALATE` | Quorum cannot resolve; escalation to human review required (triggers Escalation Trigger). |