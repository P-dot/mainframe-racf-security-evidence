# Hardening Roadmap

This roadmap is intentionally phased. It avoids applying aggressive RACF changes directly to the ADCD system without rollback.

## Phase 1 — Preserve baseline and rollback evidence

- Export current lab evidence.
- Keep screenshots, source Word documents and ZIP packages.
- Document current RACF state before every future change.
- Build rollback JCL/commands before remediation testing.

## Phase 2 — Audit logging foundation

- Review and activate SMF only in a controlled manner.
- Validate RACF event recording.
- Define which failures and successes should be logged.
- Test audit reports on non-critical resources first.

## Phase 3 — DATASET protection model using test libraries

- Create test datasets that imitate system-library naming patterns without touching real APF/system libraries.
- Define generic profiles with `UACC(NONE)`.
- Permit only controlled groups.
- Add audit settings.
- Demonstrate before/after access behavior.

## Phase 4 — OMVS / UID hardening model

- Inventory all UIDs and GIDs.
- Identify UID(0) users.
- Create a target model with unique service UIDs.
- Prefer granular `UNIXPRIV` controls over broad UID(0), where appropriate.
- Do not change real started task identities without a tested recovery path.

## Phase 5 — SAF granular controls

Prioritize:

```text
FACILITY BPX.*
UNIXPRIV
SERVAUTH EZB.*
OPERCMDS MVS.* / JES2.*
JESJOBS
JESSPOOL
SDSF
```

Build each class as a separate controlled lab.

## Phase 6 — APF / PROGRAM / LINKLIST protection

- Inventory APF list and LINKLIST.
- Protect APF libraries with RACF DATASET profiles.
- Review `PROGRAM` profiles.
- Control dynamic APF commands with `OPERCMDS`.
- Audit update access to authorized libraries.

## Phase 7 — JES2 / SDSF lockdown

- Limit SYSLOG, job output, purge/cancel and operator functions.
- Use SAF-based SDSF security where supported.
- Separate read-only monitoring from operational authority.

## Phase 8 — Continuous review

- Re-run Health Checker style reviews.
- Add zSecure-style reporting where available.
- Track deviations and exceptions.
- Maintain a monthly evidence pack.
