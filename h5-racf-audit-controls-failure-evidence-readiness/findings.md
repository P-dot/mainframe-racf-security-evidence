# Findings

## Finding 1 — AUDIT profile created safely

`IBMUSER.SECLAB.AUDIT.*` was created as a generic RACF dataset profile with `UACC(NONE)`.

Risk controlled: no production or system dataset was affected.

## Finding 2 — Failure auditing configured

The profile was configured with:

```text
AUDIT(FAILURES(READ))
```

This prepares the profile to record denied read attempts.

## Finding 3 — ALL(READ) was tested and reverted

`AUDIT(ALL(READ))` was tested for comparison, then reverted to `AUDIT(FAILURES(READ))` to avoid unnecessary audit noise.

## Finding 4 — SMF MAN datasets require attention

The `/D SMF` evidence shows SYS1.MAN1, SYS1.MAN2 and SYS1.MAN3 at 100% with `DUMP REQUIRED`.

This limits forensic readiness: RACF audit settings are not enough if system-level logging is not operationally healthy.

## Finding 5 — Interactive attacker simulation remains limited

A realistic failed access test still requires a usable non-privileged test user. Earlier evidence showed `USER2` exists but is not authorized to use TSO. `IBMUSER` is too privileged to represent a normal attacker.
