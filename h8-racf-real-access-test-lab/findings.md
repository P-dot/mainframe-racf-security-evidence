# Findings

## Finding 1 — UACC(READ) allows real read access

`H7USER` was able to browse `IBMUSER.SECLAB.PUBLIC.DATA`.

Reason:

```text
IBMUSER.SECLAB.PUBLIC.* -> UACC(READ)
```

## Finding 2 — UACC(NONE) denies unpermitted access

`H7USER` was denied access to `IBMUSER.SECLAB.PRIVATE.DATA`.

Reason:

```text
IBMUSER.SECLAB.PRIVATE.* -> UACC(NONE)
no H7USER access list entry
```

## Finding 3 — Explicit READ permit works as intended

`H7USER` was able to browse `IBMUSER.SECLAB.GRANTED.DATA`.

Reason:

```text
IBMUSER.SECLAB.GRANTED.* -> UACC(NONE)
H7USER READ
```

## Finding 4 — Audit readiness depends on profile and logging

`IBMUSER.SECLAB.AUDIT.*` is configured with:

```text
AUDIT(FAILURES(READ))
```

The profile is ready for failed-read audit, but full forensic validation also depends on the system logging/SMF state.
