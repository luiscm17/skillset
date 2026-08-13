# Verification Checklist

## Behavior Matrix

Require policy-level examples or tests for each applicable row. Express inputs as stable policy facts and expected allow/deny outcomes, not framework calls.

| Area | Evidence to require |
| --- | --- |
| Default deny | No assignment, no role, missing permission, and new scope all deny |
| Exact matching | Correct action/wrong scope and wrong action/correct scope deny; labels and routes cannot substitute |
| Unknown/malformed | Unknown IDs, incomplete permissions, invalid references, and malformed requests deny safely without widening access |
| Multiple roles | Selected composition is demonstrated; additive combinations grant only the defined union when additivity was chosen |
| Hierarchy/deny | If enabled, precedence, specificity, cycles, and conflicts have explicit expected outcomes |
| Lifecycle | Inactive, expired, suspended, future, and reactivated entities follow the selected participation rules and preserve history |
| Label independence | Renaming, localizing, or duplicating a display label leaves decisions unchanged |
| Configurable growth | New roles, scopes, actions, presets, and labels require catalog/policy changes, not evaluator changes, when semantics are unchanged |
| Trusted enforcement | Direct requests that bypass presentation receive the same authoritative decision |
| Domain boundary | Authorization can allow while domain validation rejects; neither result is mistaken for the other |
| Missing contracts | Unavailable owner/provider operations are omitted or explicitly non-interactive, never simulated |
| Shared changes | Preview reports effective-permission deltas and affected subjects, not membership alone |
| Stale administration | Concurrent mutation invalidates stale confirmation; no automatic replay occurs |
| Invariants | Selected last-admin or break-glass rules are enforced atomically; absent rules are not invented |
| Audit | Actor, target, change, before, after, and time reconstruct each mutation; reason follows the chosen policy |

## Review Prompts

- Which semantic choices are explicit, and which are still assumptions?
- What exact stable facts produce this decision?
- Can a label, route, client state, or job title alter access?
- Where is trusted enforcement, and can another entry path bypass it?
- Does any broad match, inheritance edge, deny rule, or context constraint widen authority unexpectedly?
- Does changing a shared object identify effective impact under current lifecycle and composition rules?
- Can stale state overwrite newer policy or trigger an automatic mutation replay?
- Does catalog growth preserve default deny and avoid engine edits?
- Are authentication, authorization, domain validation, presentation, and audit ownership distinct?
- Are unavailable operations honest about missing contracts?

## Output Evidence

Return:

1. A decision table listing selected semantics and unresolved choices.
2. An allow/deny table covering normal, negative, malformed, lifecycle, and combination cases.
3. Trace evidence from protected operation to stable action/resource facts and trusted enforcement.
4. Administration evidence for impact preview, freshness, conflicts, atomic invariants, and audit reconstruction.
5. A concise defect list ordered by unauthorized-access risk, lockout/governance risk, and maintainability risk.

Do not accept screenshots, hidden controls, route guards, labels, role names, or successful authentication as proof of authorization. Evidence must demonstrate policy behavior at the authoritative boundary.
