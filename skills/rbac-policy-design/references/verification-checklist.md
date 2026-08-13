# Verification Checklist

## Behavior Matrix

Require policy-level examples or tests for each applicable row. Express inputs as stable policy facts and expected allow/deny outcomes, not framework calls.

| Area | Evidence to require |
| --- | --- |
| Default deny | No assignment, no role, missing permission, and new scope all deny |
| Exact matching | Correct action/wrong scope and wrong action/correct scope deny; labels and routes cannot substitute |
| Target identity | Wrong, guessed, unknown, or omitted required instance ID denies; type-wide grants work only when explicit |
| Scope isolation | Cross-scope or cross-tenant access denies unless an explicit reviewed rule grants it |
| Unknown/malformed | Unknown IDs, incomplete permissions, invalid references, and malformed requests deny safely without widening access |
| Multiple roles | Selected composition is demonstrated; additive combinations grant only the defined union when additivity was chosen |
| Hierarchy/deny | If enabled, precedence, specificity, cycles, and conflicts have explicit expected outcomes |
| Lifecycle | Inactive, expired, suspended, future, and reactivated entities follow the selected participation rules and preserve history |
| Role activation | An assigned but inactive role does not participate when activation is required |
| SoD | Selected static conflicts reject incompatible assignments; dynamic conflicts reject prohibited concurrent or operation-time use |
| Trusted constraints | Missing, stale, or untrusted attributes cannot satisfy ABAC, ReBAC, ownership, delegation, or contextual constraints |
| Label independence | Renaming, localizing, or duplicating a display label leaves decisions unchanged |
| Configurable growth | New roles, scopes, actions, presets, and labels require catalog/policy changes, not evaluator changes, when semantics are unchanged |
| Trusted enforcement | Direct requests that bypass presentation receive the same authoritative decision |
| Domain boundary | Authorization can allow while domain validation rejects; neither result is mistaken for the other |
| Missing contracts | Unavailable owner/provider operations are omitted or explicitly non-interactive, never simulated |
| Shared changes | Preview reports effective-permission deltas and affected subjects, not membership alone |
| Stale administration | Concurrent mutation invalidates stale confirmation; no automatic replay occurs |
| Revocation/cache | A cached allow stops authorizing within the declared revocation bound; stale or unverifiable policy fails closed |
| Check-to-use | A relevant policy change between decision and protected mutation cannot preserve an obsolete allow beyond the selected guarantee |
| Bundles | Expansion uses an explicit bundle version; membership changes cannot silently add future authority |
| Migration | Rename, split, merge, deprecation, rollback, and mixed-version cases preserve intended authority and audit lineage |
| Principal kinds | Each selected anonymous, non-human, device, or delegated principal follows explicit identity, lifecycle, and provenance rules |
| Invariants | Selected last-admin or break-glass rules are enforced atomically; absent rules are not invented |
| Audit | Actor, target, change, before, after, and time reconstruct each mutation; reason follows the chosen policy |

## Review Prompts

- Which semantic choices are explicit, and which are still assumptions?
- What exact stable facts produce this decision?
- Can a label, interface state, entry-point name, or job title alter access?
- Where is trusted enforcement, and can another entry path bypass it?
- Does any broad match, inheritance edge, deny rule, or context constraint widen authority unexpectedly?
- Does changing a shared object identify effective impact under current lifecycle and composition rules?
- Can stale state overwrite newer policy or trigger an automatic mutation replay?
- Does catalog growth preserve default deny and avoid engine edits?
- Are authentication, authorization, domain validation, presentation, and audit ownership distinct?
- Are unavailable operations honest about missing contracts?

## Output Evidence

Return:

1. A semantic decision table listing selected semantics and unresolved choices.
2. An allow/deny evidence table covering applicable normal, negative, malformed, lifecycle, scope, instance, constraint, freshness, bundle, migration, and principal-kind cases.
3. Trace evidence from protected operation to stable action/resource facts and trusted enforcement.
4. Administration evidence for impact preview, freshness, conflicts, atomic invariants, and audit reconstruction.
5. A concise defect list ordered by unauthorized-access risk, lockout/governance risk, and maintainability risk.

Do not accept presentation state, entry-point guards, labels, role names, or successful identity verification as proof of authorization. Evidence must demonstrate policy behavior at the trusted policy enforcement point.
