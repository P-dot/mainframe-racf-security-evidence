# Risk Analysis

## Risk addressed

Direct user-level RACF permits can create long-term access clutter and make reviews harder. A user might retain access after moving to another role if the access list is not cleaned.

## Safer pattern

Group-based access makes entitlement review easier:

- Review group membership.
- Review group access to resources.
- Remove access by removing the user from the group or removing the group permit.

## Remaining risk

Group-based access only works well if the group itself is governed. If too many users are connected to `H7GRP`, the group becomes over-permissive.

