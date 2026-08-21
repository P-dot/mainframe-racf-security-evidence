# Lab 26 — RACF OPERCMDS Effective Authority Analysis

## Status

**COMPLETED**

Controlled z/OS security assessment. No RACF authorization changes were performed.

## Environment

- IBM z/OS ADCD 1.11
- RACF / SAF
- TSO / ISPF
- SDSF
- JES2
- Hercules laboratory environment

## Purpose

Determine the effective authority available to `IBMUSER` for operator-command activity and distinguish:

- the ability to submit commands from SDSF;
- static RACF `OPERCMDS` profile permissions;
- effective command authorization;
- warning-mode coverage;
- actual security enforcement.

This lab follows Lab 25 and resolves its open `OPERCMDS` effective-authority question before any hardening changes are introduced.

## Scope

The assessment covered:

- `SETROPTS` class status;
- inventory of `OPERCMDS` profiles;
- detailed review of `MVS.SETPROG`, `MVS.SET.PROG` and `MVS.**`;
- `IBMUSER` RACF context;
- SDSF execution context;
- safe read-only MVS command tests;
- a controlled negative authorization test using `SETPROG APF,DISPLAY`.

No `RDEFINE`, `RALTER`, `PERMIT`, `SETROPTS RACLIST(... ) REFRESH`, APF update, START, STOP, CANCEL, FORCE or VARY change was performed.

## Observed OPERCMDS baseline

| Profile | Type | UACC | IBMUSER | WARNING | Audit |
|---|---|---|---|---|---|
| `MVS.SETPROG` | specific | `NONE` | `READ` | `NO` | `FAILURES(READ)` |
| `MVS.SET.PROG` | specific | `NONE` | `READ` | `NO` | `FAILURES(READ)` |
| `MVS.**` | generic | `NONE` | `READ` | `YES` | `FAILURES(READ)` |

The generic `MVS.**` profile therefore remains a controlled hardening candidate. Its `WARNING` status must be evaluated before enforcement is tightened.

## Effective-authority validation

The following read-only commands were accepted from SDSF:

```text
/D A,L
/D IPLINFO
/D OPDATA
```

The controlled `SETPROG` query was rejected:

```text
/SETPROG APF,DISPLAY
```

Observed security message:

```text
IEE345I SETPROG AUTHORITY INVALID, FAILED BY SECURITY
```

This is a successful negative test: the command path was available, but effective security enforcement denied the requested administrative operation.

## SDSF execution context

`WHO` confirmed the test session as:

```text
USERID=IBMUSER
GRPNAME=ISFSPROG
MVS=z/OS 01.11.00
JES=z/OS 1.11
ISFNAME=JES2
MEMBER=SYS1
JESTYPE=JES2
SYSNAME=ADCD
SYSPLEX=ADCDPL
```

## Key conclusion

The ability to submit MVS commands from SDSF does **not** imply unrestricted operator authority.

Static `READ` access displayed in an `OPERCMDS` profile also must not be interpreted in isolation as proof that every related operation is executable. The `SETPROG` test demonstrated effective security enforcement by rejecting the operation with `IEE345I`.

## Finding

### F26-01 — Generic OPERCMDS WARNING exposure

**Classification:** REVIEW / HARDEN CANDIDATE

Observed:

- `OPERCMDS` is active.
- Specific `MVS.SETPROG` and `MVS.SET.PROG` profiles are enforced with `UACC(NONE)` and `WARNING(NO)`.
- The generic `MVS.**` profile has `UACC(NONE)` but remains in `WARNING`.
- Selected display commands are operational.
- A `SETPROG` operation was explicitly rejected by security.

Risk:

A broad generic operator-command profile remaining in warning mode can prevent the installation from treating its entire `MVS.*` command-control surface as fully enforced.

Decision:

Do **not** remove `WARNING` during this lab. The next phase must include impact analysis, rollback planning, controlled hardening and before/after validation.

## Final status

**LAB 26: COMPLETED**

- Effective authority assessed.
- Positive and negative command tests captured.
- No uncontrolled RACF changes performed.
- Hardening candidate identified for Lab 27.
