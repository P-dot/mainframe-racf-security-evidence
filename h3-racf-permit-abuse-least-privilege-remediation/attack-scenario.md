# Attack Scenario — Excessive Access List Permission

## Scenario

An attacker or over-privileged user does not need a profile to have open `UACC(READ)` if they already appear in the access list with excessive authority.

In this lab the risky state is simulated with:

```text
PERMIT 'IBMUSER.SECLAB.GRANTED.*' CLASS(DATASET) ID(USER2) ACCESS(ALTER)
```

## Why this matters

A beginner might see:

```text
UNIVERSAL ACCESS: NONE
```

and conclude that the profile is secure.

A RACF security engineer also checks:

```text
ID      ACCESS
USER2   ALTER
```

## Risk model

`ALTER` is excessive when the user only needs to read data. In a real system, unnecessary ALTER authority can increase the impact of credential misuse, insider error, script mistakes, or unauthorized operational activity.

## Safe boundary

The lab is non-destructive because it only uses the sandbox profile:

```text
IBMUSER.SECLAB.GRANTED.*
```
