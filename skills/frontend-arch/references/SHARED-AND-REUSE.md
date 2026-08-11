# Shared Code and Reuse

Shared code is an explicit ownership decision, not a destination for anything used twice. Keep it intentionally small, technical or genuinely universal, and independent of capability-specific policy.

## Extraction Gates

Extract only when evidence shows:

- at least two proven consumers;
- materially equivalent semantics and lifecycle;
- repeated change pressure reduced by one owner;
- an explicit owner and evolution policy;
- a narrow contract and independent verification surface.

Visual, syntactic, or structural similarity is insufficient. Similar interface elements may express different product meaning, permissions, state, or evolution. Prefer local duplication when it protects capability autonomy.

## What May Be Shared

Technical mechanisms with stable semantics may be shared, as may genuinely universal product concepts governed by one explicit owner. Reusable visual primitives may be shared when their contract is presentation mechanics rather than hidden capability policy. Compositions that encode capability meaning stay with that capability even when visually similar.

| Evidence | Prefer |
| --- | --- |
| Same mechanism, different product meaning | Share only the mechanism; keep policy local. |
| Same meaning and lifecycle with explicit common ownership | Consider a narrow shared contract. |
| One capability owns the concept | Other capabilities consume its public contract or translate locally. |
| Consumers evolve independently | Prefer duplication or translation. |
| Ownership is unclear | Do not extract. |

Review shared code for owner, semantic scope, exclusions, compatibility burden, capability-policy leakage, new-consumer impact, and an exit path if meanings diverge.
