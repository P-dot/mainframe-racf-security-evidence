# H7 — RACF Test Identity Lab

## Objective

Create a controlled, non-privileged RACF test identity that can be used in later labs to generate real access successes, denials, and audit-relevant security events without using `IBMUSER` as the test actor.

## Why this lab matters

Previous labs used `IBMUSER`, which has excessive authority for realistic security testing. `USER2` existed, but it was not usable for TSO logon in the prior evidence. This lab creates `H7USER` as a controlled RACF identity with a dedicated lab group and a working TSO segment.

## Final state

- `H7GRP` exists as a RACF lab group.
- `H7USER` exists as a RACF lab user.
- `H7USER` default group is `H7GRP`.
- No global privileged attributes are visible for `H7USER` in the captured evidence.
- `H7USER` has a TSO segment using `DBSPROC9` and `COMMAND(ISPF)`.
- `H7USER` is authorized to use `ACCT#` through the `ACCTNUM` class.
- `H7USER` successfully reaches ISPF.
- `H7USER` has `READ` access to `IBMUSER.SECLAB.GRANTED.*`.

## Scope control

This lab does not grant `SPECIAL`, `OPERATIONS`, `AUDITOR`, `UID(0)`, access to production datasets, STARTED authority, APF authority, or OPERCMDS authority.

## What comes next

The next lab can use `H7USER` to test actual RACF allow/deny behavior against the sandbox profiles:

- `IBMUSER.SECLAB.PUBLIC.*`
- `IBMUSER.SECLAB.PRIVATE.*`
- `IBMUSER.SECLAB.GRANTED.*`
- `IBMUSER.SECLAB.AUDIT.*`
