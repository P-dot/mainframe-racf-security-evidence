# Risk Analysis — Lab 28

## Scope

This is a controlled RACF training exercise. The changes are limited to a dedicated lab group/profile namespace.

## Controls applied

- Dedicated lab group: `RACFL28`.
- Group authority limited to `USE` for the connection under test.
- No group-SPECIAL, group-OPERATIONS or group-AUDITOR granted.
- Generic DATASET profile configured with `UACC(NONE)`.
- Warning mode not used for the final profile.
- Failed accesses audited from READ.
- Access granted through a group entry rather than a direct per-user entry.
- No global EGN change made merely to satisfy the exercise.

## Residual risks / limitations

### Privileged test identity

`IBMUSER` is privileged in this ADCD environment. A successful resource operation cannot be attributed solely to the group access-list entry without a non-privileged comparison identity.

### ALTER authority

`ALTER` is powerful. It is acceptable here because the profile is a dedicated training namespace and the lab includes dataset creation/management. In production, the required access should be derived from actual job duties and reduced where possible.

### Generic profile naming

The system has generic DATASET profiles enabled but Enhanced Generic Naming is not active. The lab preserves that global state and documents the naming behavior instead of changing installation-wide semantics.

## Production recommendation

Use dedicated non-privileged test identities, formal change control, peer review, explicit rollback planning, SMF review, and narrowly scoped group authorities before applying comparable changes in production.
