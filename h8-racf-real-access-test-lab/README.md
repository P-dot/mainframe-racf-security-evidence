# H8 — RACF Real Access Test Lab

## Objective

Validate RACF dataset access using a real non-privileged test identity: `H7USER`.

This lab moves from profile review to real access behaviour:

- `PUBLIC` should be readable through `UACC(READ)`.
- `PRIVATE` should be denied because it is closed with `UACC(NONE)`.
- `GRANTED` should be readable because `H7USER` has explicit `READ`.
- `AUDIT` should remain closed and configured for failure audit readiness.

## Scope

Objects used:

```text
IBMUSER.SECLAB.PUBLIC.*
IBMUSER.SECLAB.PRIVATE.*
IBMUSER.SECLAB.GRANTED.*
IBMUSER.SECLAB.AUDIT.*
H7USER
```

Out of scope:

```text
SYS1.*
ADCD.Z111S.*
STARTED
APF
LINKLIST
DB2
CICS
OPERCMDS changes
```

## Result

The lab successfully demonstrated real access control with `H7USER`:

```text
PUBLIC.DATA   -> browse success
PRIVATE.DATA  -> access denied / ICH408I evidence
GRANTED.DATA  -> browse success through explicit permit
AUDIT profile -> restricted and configured with AUDIT(FAILURES(READ))
```

## Professional Value

This lab demonstrates that RACF security must be validated with a realistic test identity, not only by listing profiles from a privileged account.
