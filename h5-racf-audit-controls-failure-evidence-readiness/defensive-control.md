# Defensive Control

The defensive control demonstrated here is RACF dataset audit configuration.

## Baseline protection

```text
UACC(NONE)
```

This means the profile is closed by default.

## Failure auditing

```text
AUDIT(FAILURES(READ))
```

This is useful when the expected access pattern is small and the security team mainly wants to know about denied read attempts.

## Full read auditing

```text
AUDIT(ALL(READ))
```

This captures both successful and failed read attempts, but it can be noisier.

## Final lab posture

The lab returns to:

```text
AUDIT(FAILURES(READ))
```

This preserves a focused defensive signal without leaving the profile in a noisier state.
