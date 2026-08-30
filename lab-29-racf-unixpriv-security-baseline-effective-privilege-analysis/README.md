# Lab 29 — RACF UNIXPRIV Security Baseline & Effective Privilege Analysis

## Objective

Establish a read-only RACF/SAF security baseline for privileged z/OS UNIX authorization, focusing on the `UNIXPRIV` class, existing `SUPERUSER.FILESYS*` profiles, their effective access lists, and the presence of UID(0) mappings.

This lab deliberately separates **RACF security control** from general USS administration. It does not teach shell commands, `chmod`, `chown`, filesystem administration, or zFS operations.

## Scope and safety

- Environment: controlled z/OS ADCD / Hercules laboratory.
- Baseline and inquiry only.
- No `PERMIT`, `RDEFINE`, `RALTER`, `ALTUSER`, `SETROPTS` changes, UID changes, or service-account changes were performed.
- No technical UID(0) identity was modified.
- `IBMUSER` was used only as the existing administrative identity during inquiry.
- `H7USER` is retained as the repository's controlled non-privileged test identity for later change/validation labs; it is **not modified in Lab 29**.

## Baseline results

### UNIXPRIV class status

The RACF system-options display showed `UNIXPRIV` as:

- ACTIVE
- GENERIC
- RACLISTed

Therefore the installation already has the RACF infrastructure required to process and cache UNIXPRIV profiles.

### UNIXPRIV profile inventory

A RACF General Resource Profile search returned four profiles:

1. `SUPERUSER.FILESYS`
2. `SUPERUSER.FILESYS.CHANGEPERMS`
3. `SUPERUSER.FILESYS.CHOWN`
4. `SUPERUSER.FILESYS.MOUNT`

### Effective authorization matrix

| UNIXPRIV profile | UACC | Warning | Auditing | Explicit access |
|---|---|---|---|---|
| SUPERUSER.FILESYS | NONE | NO | FAILURES(READ) | IBMUSER ALTER |
| SUPERUSER.FILESYS.CHANGEPERMS | NONE | NO | FAILURES(READ) | IBMUSER ALTER |
| SUPERUSER.FILESYS.CHOWN | NONE | NO | FAILURES(READ) | IBMUSER ALTER |
| SUPERUSER.FILESYS.MOUNT | NONE | NO | FAILURES(READ) | IBMUSER ALTER |

The profiles are not universally open. `UACC(NONE)` establishes a closed default, while `IBMUSER` has explicit authority on all four profiles.

### UID(0) inventory attempt

The RACF USER SEARCH panel exposes a `UID` criterion. A search for `UID=0` returned:

`ICH31028I THE UID KEYWORD REQUIRES APPLICATION IDENTITY MAPPING TO BE IMPLEMENTED.`

This is retained as troubleshooting evidence. No global RACF configuration was changed merely to make the query work.

### Compatible alternative: UNIXMAP

The read-only query:

`RLIST UNIXMAP U0 ALL`

successfully displayed the existing `UNIXMAP` profile `U0`, demonstrating that multiple technical/administrative identities are associated with the UID(0) mapping in this ADCD environment. Visible examples include `PAGENT`, `IBMUSER`, `BPXOINIT`, `INETD`, `FTPD`, `TCPIP`, `WEBSRV`, `DSNWLM1`, `START1`, `START2`, and `OPEN1`–`OPEN3`.

The evidence is intentionally treated as an **exposure inventory**, not as proof that any listed technical identity can safely be converted away from UID(0).

## Findings

### F29-01 — UNIXPRIV is operationally enabled

`UNIXPRIV` is active, generic-profile enabled, and RACLISTed.

**Assessment:** positive control foundation.

### F29-02 — Privileged filesystem capabilities are protected by explicit RACF profiles

Four `SUPERUSER.FILESYS*` profiles exist with `UACC(NONE)` and `WARNING(NO)`.

**Assessment:** positive default-deny posture.

### F29-03 — Privileged UNIX authorization is concentrated in IBMUSER

All four observed profiles explicitly grant `IBMUSER ALTER`.

**Assessment:** expected in a training ADCD administrative account, but not a least-privilege production model.

### F29-04 — Multiple UID(0) mappings remain present

`UNIXMAP U0` contains multiple system/technical identities.

**Assessment:** significant hardening review area. Do not remove UID(0) from technical identities without dependency analysis, rollback planning, and controlled validation.

### F29-05 — UID search is constrained by the installation's identity-mapping level

The native USER SEARCH `UID=0` attempt produced `ICH31028I`.

**Assessment:** documented platform/configuration limitation; the lab used a compatible read-only alternative instead of changing global RACF configuration.

## Security conclusion

The system already contains a granular RACF `UNIXPRIV` framework, but it coexists with multiple UID(0) mappings. This makes the next controlled step a least-privilege validation using the existing non-privileged `H7USER` laboratory identity rather than modifying critical IDs such as `TCPIP`, `BPXOINIT`, `PAGENT`, or other service identities.

## Next lab

**Lab 30 — RACF UNIXPRIV Controlled Delegation & Validation**

Use `H7USER` as the controlled actor to test a narrowly scoped UNIXPRIV authorization with before/deny → controlled grant → refresh if required → after/allow → rollback evidence. Critical technical identities remain untouched.
