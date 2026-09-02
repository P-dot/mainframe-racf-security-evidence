# Lab 30 — RACF UNIXPRIV Controlled Delegation & Validation

## Objective
Demonstrate least-privilege delegation in z/OS UNIX through RACF/SAF `UNIXPRIV`, using a controlled non-privileged identity (`H7USER`) instead of UID(0), `SPECIAL`, or `OPERATIONS`.

The lab proves the full authorization lifecycle with the **same user, same file, and same operation**:

`DENIED → minimal RACF grant → ALLOWED → rollback → DENIED`

## Scope
This lab focuses on RACF/SAF security over z/OS UNIX. USS is used only to perform the minimum functional operation required to prove effective authorization.

### Identities
- `IBMUSER`: administrative actor used to inspect and change RACF configuration and prepare the controlled test object.
- `H7USER`: controlled test subject. It remains non-privileged globally.
- `H7GRP`: RACF group associated with the test identity.

## Initial baseline
Before the functional UNIXPRIV test:
- `H7USER` had no global `SPECIAL`, `OPERATIONS`, or `AUDITOR` authority.
- `H7USER` was not assigned UID(0).
- `H7GRP` initially had no OMVS information.
- `H7USER` initially had no usable OMVS segment.
- `UNIXPRIV` was active and RACLISTed.
- `SUPERUSER.FILESYS.CHANGEPERMS` existed with `UACC(NONE)` and no authorization for `H7USER`.

## Phase 1 — Prepare a non-privileged UNIX identity
A candidate GID was checked with:

```text
RLIST UNIXMAP G1000 ALL
```

`G1000` was not found, so `H7GRP` was assigned GID 1000:

```text
ALTGROUP H7GRP OMVS(GID(1000))
LISTGRP H7GRP OMVS
```

A candidate UID was then checked:

```text
RLIST UNIXMAP U1000 ALL
```

`U1000` was not found. `H7USER` was therefore prepared as a normal UNIX identity:

```text
ALTUSER H7USER OMVS(UID(1000) HOME('/u/h7user') PROGRAM('/bin/sh'))
LISTUSER H7USER OMVS
```

This does **not** make H7USER a UNIX superuser. UID 1000 is deliberately nonzero.

## Phase 2 — Troubleshooting the HOME dependency
The first OMVS start by H7USER failed with `FSUM2384`. RACF contained `HOME=/u/h7user`, but that path did not yet exist in the UNIX filesystem.

The important distinction is:

- RACF `HOME('/u/h7user')` records the user's home path.
- It does **not** create that directory in USS.

IBMUSER therefore created only the required lab HOME:

```sh
mkdir /u/h7user
ls -ld /u/h7user
```

After a fresh H7USER session, OMVS started successfully and the UID/GID context could be validated.

See `docs/troubleshooting-fsum2384.md`.

## Phase 3 — Establish the UNIXPRIV BEFORE baseline
The target profile was inspected before any delegation:

```text
RLIST UNIXPRIV SUPERUSER.FILESYS.CHANGEPERMS ALL
```

`H7USER` was not present in the access list.

IBMUSER created a controlled file:

```sh
cd /tmp
touch lab30_ibmuser.txt
chmod 600 lab30_ibmuser.txt
ls -l lab30_ibmuser.txt
```

H7USER then attempted to change permissions on the IBMUSER-owned file:

```sh
ls -l /tmp/lab30_ibmuser.txt
chmod 644 /tmp/lab30_ibmuser.txt
ls -l /tmp/lab30_ibmuser.txt
```

Result: **DENIED**. The file permissions remained unchanged.

## Phase 4 — Minimal UNIXPRIV delegation
IBMUSER granted only the access required for the specific capability:

```text
PERMIT SUPERUSER.FILESYS.CHANGEPERMS CLASS(UNIXPRIV) ID(H7USER) ACCESS(READ)
SETROPTS RACLIST(UNIXPRIV) REFRESH
RLIST UNIXPRIV SUPERUSER.FILESYS.CHANGEPERMS ALL
```

The resulting ACL showed `H7USER` with `READ` access.

No UID(0), `SPECIAL`, `OPERATIONS`, broad UNIXPRIV wildcard, or global superuser authority was granted.

## Phase 5 — Effective authorization test
H7USER repeated the same operation against the same file:

```sh
ls -l /tmp/lab30_ibmuser.txt
chmod 644 /tmp/lab30_ibmuser.txt
ls -l /tmp/lab30_ibmuser.txt
```

Result: **ALLOWED**. The mode changed from 600 to 644.

This isolates the RACF UNIXPRIV authorization as the changed security variable.

## Phase 6 — Rollback
IBMUSER removed only H7USER's explicit authorization and refreshed the RACLISTed class:

```text
PERMIT SUPERUSER.FILESYS.CHANGEPERMS CLASS(UNIXPRIV) ID(H7USER) DELETE
SETROPTS RACLIST(UNIXPRIV) REFRESH
RLIST UNIXPRIV SUPERUSER.FILESYS.CHANGEPERMS ALL
```

The verification showed H7USER no longer present in the ACL.

H7USER then attempted another permission change:

```sh
ls -l /tmp/lab30_ibmuser.txt
chmod 600 /tmp/lab30_ibmuser.txt
ls -l /tmp/lab30_ibmuser.txt
```

Result: **DENIED** again. The mode remained 644.

## Final result

| Stage | H7USER UNIXPRIV access | Operation | Result |
|---|---|---|---|
| BEFORE | None | `chmod` IBMUSER-owned file | DENIED |
| Delegated | READ to `SUPERUSER.FILESYS.CHANGEPERMS` | same controlled `chmod` | ALLOWED |
| Rollback | Explicit access removed | controlled `chmod` again | DENIED |

### Security conclusion
The lab demonstrates that z/OS UNIX privileged functions can be delegated through SAF/RACF at a much narrower scope than granting UID(0) or broad RACF administrative attributes. Effective access was demonstrated functionally and then removed, with a second functional denial proving rollback effectiveness.

## Repository contents
- `README.md` — complete lab narrative.
- `commands.md` — commands separated by administrative actor and test subject.
- `findings.md` — observations and security findings.
- `risk-analysis.md` — least-privilege and residual-risk analysis.
- `rollback.md` — rollback procedure and final state.
- `docs/troubleshooting-fsum2384.md` — HOME/OMVS startup incident.
- `docs/technical-notes.md` — identity, RACLIST, and test-design notes.
- `evidence/evidence-manifest.md` — evidence description.
- `evidence/screenshots/` — screenshots supplied during execution.

## Result
**LAB STATUS: COMPLETE — functional DENIED → ALLOWED → DENIED chain validated.**
