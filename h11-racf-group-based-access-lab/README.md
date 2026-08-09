# H11 — RACF Group-Based Access Lab

## Objective

Validate RACF group-based access using a safe laboratory sandbox under `IBMUSER.SECLAB.*`.

This lab moves from individual user permissions to a role/group model:

- `H7USER` is the non-privileged test identity created in H7.
- `H7GRP` is the laboratory RACF group.
- `IBMUSER.SECLAB.GROUP.*` is the protected dataset profile.
- Access is granted to `H7GRP`, not directly to `H7USER`.

## Professional security idea

In a production RACF model, permissions should normally be administered through groups/roles instead of many direct user-specific access list entries. A user receives access because they belong to the correct group, and removing the group access removes the inherited access path.

## Final lab status

The evidence shows:

- `H7USER` exists and is connected to `H7GRP`.
- `IBMUSER.SECLAB.GROUP.DATA` was cataloged and populated with benign test content.
- `IBMUSER.SECLAB.GROUP.*` was protected with `UACC(NONE)` and `AUDIT(FAILURES(READ))`.
- `H7GRP READ` was added to the access list.
- The group access entry was later removed.
- Final profile state returned to `UACC(NONE)` with no standard access list entries.

## Evidence limitation

The RACF profile lifecycle is clearly captured. Some H7USER denial screenshots in the source evidence show `IBMUSER.SECLAB.PRIVATE.DATA` rather than `IBMUSER.SECLAB.GROUP.DATA`; therefore, this lab does **not** overstate those screenshots as proof of a GROUP.DATA denial. They are retained as contextual evidence of the same non-privileged test identity and RACF violation pattern.

## Safety boundaries

No production/system resources were modified:

- No `SYS1.*`
- No `ADCD.Z111S.*`
- No APF/LINKLIST changes
- No STARTED profile changes
- No DB2/CICS changes
- No `SPECIAL`, `OPERATIONS`, or `AUDITOR` attributes added to `H7USER`

