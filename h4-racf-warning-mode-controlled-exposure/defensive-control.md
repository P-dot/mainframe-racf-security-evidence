# Defensive Control

## Correct final control

The final state should be:

```text
UACC(NONE)
WARNING(NO)
AUDIT(ALL(READ))
```

## Why

- `UACC(NONE)` closes universal access.
- `WARNING(NO)` makes the profile enforce normally.
- `AUDIT(ALL(READ))` documents read attempts in the lab profile.

## Production interpretation

In production, `WARNING` can be useful during a staged migration:

1. Define the profile.
2. Put it in warning mode temporarily.
3. Observe who would fail.
4. Build a justified access list.
5. Disable warning mode.
6. Enforce the profile.

The mistake is leaving `WARNING` enabled permanently.
