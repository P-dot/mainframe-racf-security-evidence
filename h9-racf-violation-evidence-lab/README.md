# H9 — RACF Violation Evidence Lab

## Objective

This lab demonstrates how a controlled RACF access violation can be correlated with operational evidence in SDSF SYSLOG.

The lab uses the non-privileged test identity created in H7, `H7USER`, to attempt controlled access to sandbox datasets under `IBMUSER.SECLAB.*`.

## Scope

This lab covers:

- RACF access violation evidence.
- `ICH408I` interpretation.
- USER / GROUP / CLASS / DATASET / ACCESS INTENT / ACCESS ALLOWED fields.
- Comparison between denied and explicitly permitted access.
- SMF status as a forensic-readiness limitation.

## Safety Boundary

No production datasets were modified.

The lab did not modify:

- `SYS1.*`
- `ADCD.Z111S.*`
- STARTED profiles
- APF / LINKLIST
- DB2 / CICS
- RACF database structure
- global auditing policy

All access testing was performed against the existing RACF sandbox.

## Lab Summary

`H7USER` was used as the controlled non-privileged identity. The lab verified that private and audit-protected sandbox datasets deny access, while a granted dataset allows access because of an explicit `H7USER READ` access list entry.

The violation evidence was captured from SDSF SYSLOG through `ICH408I` messages.

## Key Result

The lab proves that RACF can be tested safely using a non-privileged identity and that denied access can be tied back to a concrete RACF message.

```text
H7USER + PRIVATE.DATA -> denied, ICH408I evidence
H7USER + GRANTED.DATA -> permitted, H7USER READ access list
H7USER + AUDIT.DATA   -> denied profile prepared with AUDIT(FAILURES(READ))
```

## Evidence

Screenshots are located in:

```text
evidence/screenshots/
```

The contact sheet is located in:

```text
evidence/h9_contact_sheet.jpg
```
