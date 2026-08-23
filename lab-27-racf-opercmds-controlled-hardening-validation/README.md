# Lab 27 — RACF OPERCMDS Controlled Hardening & Validation

**Status: COMPLETED — HARDENING VALIDATED**

## Objective
Continue Lab 26 by removing warning mode from the generic `OPERCMDS` profile `MVS.**`, then prove through BEFORE/AFTER testing that the selected legitimate display operations remain available while the existing SETPROG security denial remains enforced.

## Environment
IBM z/OS ADCD 1.11 · RACF/SAF · TSO/ISPF · SDSF/JES2 · Hercules.

## BEFORE baseline
`RLIST OPERCMDS MVS.** ALL` showed `UACC(NONE)`, `IBMUSER READ`, `WARNING YES`, and `FAILURES(READ)` auditing.

| Test | BEFORE |
|---|---|
| `D A,L` | Allowed |
| `D IPLINFO` | Allowed |
| `D OPDATA` | Allowed |
| `SETPROG APF,DISPLAY` | Denied: `IEE345I ... FAILED BY SECURITY` |

## Controlled change
```text
RALTER OPERCMDS MVS.** NOWARNING
```
`RALTER` modifies an existing RACF resource profile. `OPERCMDS` is the class under test, `MVS.**` is the generic profile, and `NOWARNING` removes warning mode. UACC and the documented IBMUSER access entry were not changed.

RACF reported that RACLISTed OPERCMDS profiles would not reflect the update until a refresh. Therefore:
```text
SETROPTS RACLIST(OPERCMDS) REFRESH
```
was issued to refresh the RACLISTed class.

## Verification
A new:
```text
RLIST OPERCMDS MVS.** ALL
```
confirmed `WARNING NO`, while `UACC(NONE)` and `IBMUSER READ` remained.

## AFTER validation
| Test | BEFORE | AFTER | Assessment |
|---|---|---|---|
| `D A,L` | Allowed | Allowed | PASS |
| `D IPLINFO` | Allowed | Allowed | PASS |
| `D OPDATA` | Allowed | Allowed | PASS |
| `SETPROG APF,DISPLAY` | Denied | Denied | PASS |
| `MVS.** WARNING` | YES | NO | HARDENED |

The final SETPROG test again returned `IEE345I SETPROG AUTHORITY INVALID, FAILED BY SECURITY`.

## Rollback
Prepared but not executed:
```text
RALTER OPERCMDS MVS.** WARNING
SETROPTS RACLIST(OPERCMDS) REFRESH
```
Rollback was unnecessary because the documented regression/security tests passed.

## Conclusion
The generic OPERCMDS profile was moved from warning mode to normal enforcement and the change was refreshed and verified. Within the explicitly tested scope, the selected read-only MVS display operations remained functional and the existing SETPROG denial remained effective.

## Scope limitation
This lab does not claim that every MVS operator command matching `MVS.**` was regression-tested. Conclusions are limited to the commands and evidence documented here.
