# Lab 05 - Started Task Security Review

## Objective

Review how z/OS started tasks are represented in RACF and how they actually run in SDSF. The lab compares RACF `STARTED` class definitions with active runtime owners shown in SDSF `DA`.

This is a security baseline lab. It does **not** modify RACF, started task definitions, parmlib, proclib, or system libraries.

## Environment

- Platform: ADCD z/OS 1.11 training system
- Security product: RACF
- User: IBMUSER
- Interfaces used: ISPF option 6, SDSF DA, SDSF SYSLOG/LOG

## Main security idea

Started tasks often run core services such as TCP/IP, SDSF, SSHD, FTPD, DB2, CICS, MQ and JES-related components. If these services run under overly broad technical users, or if the identity mapping is unclear, the system has a privileged identity governance risk.

The audit compares three layers:

1. RACF `STARTED` class profile.
2. `STDATA` segment, if present.
3. Runtime owner shown by SDSF `DA`.

## Key findings

- `HZSPROC` has an exact `STARTED` profile, but no visible `STDATA` segment in the captured output.
- `FTPD.*` has a generic `STARTED` profile, but no visible `STDATA` segment.
- `TCPIP.*` has a generic `STARTED` profile, but no visible `STDATA` segment.
- `SDSF`, `SSHD4`, `DB9*`, and `CICS*` were not found using targeted `SEARCH CLASS(STARTED)` patterns.
- SDSF `DA` shows several services running under specific technical users such as `TCPIP`, `FTPD`, `WEBSRV`, and `OMVSKERN`.
- SDSF `DA` also shows many critical started tasks running under generic users such as `START1` and `START2`.

## Safety statement

No changes were made. The lab is read-only.

Commands intentionally avoided:

```text
RDEFINE STARTED ...
RALTER STARTED ...
PERMIT ...
ALTUSER ...
ADDUSER ...
SETROPTS RACLIST(STARTED) REFRESH
```
