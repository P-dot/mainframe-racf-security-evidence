# H12 — RACF Access Cleanup & Review Lab

## Purpose

This lab documents a controlled RACF access-list cleanup in the `IBMUSER.SECLAB.*` sandbox.

The goal is not to delete users, profiles, or datasets. The goal is to review existing sandbox profiles, identify obsolete explicit access, remove the unnecessary access-list entry, refresh generic DATASET profiles, and verify that the legitimate test identity still works.

## Scope

In scope:

- `IBMUSER.SECLAB.PUBLIC.*`
- `IBMUSER.SECLAB.PRIVATE.*`
- `IBMUSER.SECLAB.GRANTED.*`
- `IBMUSER.SECLAB.AUDIT.*`
- `IBMUSER.SECLAB.WARNING.*`
- `IBMUSER.SECLAB.GROUP.*`
- `USER2` only as an obsolete access-list entry
- `H7USER` as the working non-privileged test identity

Out of scope:

- `SYS1.*`
- `ADCD.Z111S.*`
- STARTED profiles
- OPERCMDS hardening
- APF/LINKLIST
- DB2/CICS
- deleting users, groups, datasets, or RACF profiles

## Summary

The lab reviewed the sandbox DATASET profiles and cleaned obsolete direct access for `USER2` from `IBMUSER.SECLAB.GRANTED.*`.

Before cleanup, `IBMUSER.SECLAB.GRANTED.*` had:

```text
USER2   READ
H7USER  READ
```

After cleanup, it had:

```text
H7USER  READ
```

`H7USER` was then used to verify that legitimate access to `IBMUSER.SECLAB.GRANTED.DATA` still worked, while `IBMUSER.SECLAB.PRIVATE.DATA` remained denied and generated `ICH408I`.

## Main result

RACF cleanup was performed without deleting the sandbox profile, without opening `UACC`, and without breaking the legitimate `H7USER` access path.
