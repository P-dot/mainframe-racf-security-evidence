# Defensive Control — Closed-by-Default RACF Dataset Profiles

## Control Objective

The defensive objective is to reduce accidental exposure by making `NONE` the default and granting only required access explicitly.

## Defensive Pattern

```text
ADDSD 'IBMUSER.SECLAB.PRIVATE.*' UACC(NONE)
```

This closes the resource by default.

## Least-Privilege Pattern

```text
ADDSD 'IBMUSER.SECLAB.GRANTED.*' UACC(NONE)
PERMIT 'IBMUSER.SECLAB.GRANTED.*' CLASS(DATASET) ID(USER2) ACCESS(READ)
```

This keeps the profile closed to everyone else while allowing a specific user to read.

## Audit Pattern

```text
ALTDSD 'IBMUSER.SECLAB.PRIVATE.*' GENERIC AUDIT(FAILURES(READ))
```

This records failed read attempts against the private profile, assuming the system's audit and SMF configuration is collecting the relevant records.

## Important Production Note

In production, this pattern must be deployed through change control. Do not blindly change dataset profiles for system libraries, APF libraries, JES resources, OMVS/zFS datasets, or product libraries without impact analysis and rollback.
