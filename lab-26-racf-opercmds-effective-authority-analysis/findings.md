# Lab 26 — Findings

## F26-01 — Generic MVS.** remains in WARNING

**Status:** REVIEW / HARDEN CANDIDATE

The `OPERCMDS` class is active and contains both specific and generic profiles.

Specific profiles `MVS.SETPROG` and `MVS.SET.PROG` were observed with:

- UACC(NONE)
- IBMUSER READ
- WARNING(NO)
- FAILURES(READ) auditing

Generic profile `MVS.**` was observed with:

- UACC(NONE)
- IBMUSER READ
- WARNING(YES)
- FAILURES(READ) auditing

The generic warning profile requires controlled hardening analysis before enforcement changes.

## F26-02 — SDSF command submission is not unrestricted authority

Read-only commands `D A,L`, `D IPLINFO` and `D OPDATA` returned valid system output.

`SETPROG APF,DISPLAY` returned:

`IEE345I SETPROG AUTHORITY INVALID, FAILED BY SECURITY`

The negative test demonstrates that the presence of an SDSF command interface does not bypass security enforcement.

## F26-03 — Static profile access is not sufficient evidence of effective command authority

`IBMUSER` displayed `READ` access to the reviewed specific `OPERCMDS` profiles, yet the controlled `SETPROG` operation was denied.

Effective-authority assessment must therefore combine configuration review with controlled execution evidence.
