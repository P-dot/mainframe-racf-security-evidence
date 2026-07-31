# Findings

## Finding 1 — WARNING mode is not final hardening

The profile `IBMUSER.SECLAB.WARNING.*` was placed into warning mode and then restored.

Evidence shows the transition:

```text
WARNING: NO
WARNING: YES
WARNING: NO
```

## Finding 2 — UACC(NONE) must be read together with WARNING status

`UACC(NONE)` alone is not enough to understand enforcement.

A RACF profile review must include:

```text
UNIVERSAL ACCESS
WARNING
AUDITING
ACCESS LIST
```

## Finding 3 — Audit settings were applied to the sandbox profile

The profile was configured with:

```text
AUDIT(ALL(READ))
```

This improves traceability for the lab profile.

## Finding 4 — Final sandbox state is safe

The warning profile was not left in warning mode.

Final state:

```text
IBMUSER.SECLAB.WARNING.* -> UACC(NONE), WARNING(NO), AUDIT(ALL(READ))
```
