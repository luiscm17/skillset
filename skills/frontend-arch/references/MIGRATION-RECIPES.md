# Bounded Migration Recipes

Start from evidence, preserve behavior, and make the smallest reversible change. Every parallel path needs one transition direction and explicit retirement proof.

## Move a Capability Slice

**Evidence:** One product change is scattered across technical buckets or ownership is unclear.

1. Select one cohesive behavior and characterize it.
2. Name its canonical capability owner and public promise.
3. Move policy, presentation, state, and adapters only where they belong to that behavior.
4. Redirect consumers through the public entry point.
5. Remove confirmed orphaned paths after verification.

Do not reorganize the whole system to make the tree symmetrical.

## Replace Internal Reach-Through

1. Record the consumer's actual semantic need.
2. Add or reshape the provider's narrow public contract.
3. Migrate one consumer and verify both sides.
4. Block the internal dependency with the strongest practical enforcement.
5. Remove compatibility access when no required consumer remains.

## Consolidate Canonical State

1. Identify competing writers and stale transitions.
2. Select the owner whose lifecycle and semantics define the value.
3. Change secondary copies into derivations or projections.
4. Migrate reads before removing duplicate writes.
5. Verify that contradictory states can no longer occur.

## Extract or Return Shared Code

To extract, prove all gates in `SHARED-AND-REUSE.md`, assign ownership, and migrate consumers incrementally. To return code, separate capability-specific policy from reusable mechanics, move policy to each owner, and retain only a cohesive shared contract.

## Bound Every Transition

Define source and destination owner, one direction, covered population, compatibility and rollback bounds, prevention of competing authority, verification at each phase, retirement condition, and evidence of removal. Reject bidirectional synchronization unless an unavoidable constraint is documented and time-bounded.
