# Lab 25 — RACF FACILITY / OPERCMDS Security Review

## Purpose

Review selected security-sensitive RACF FACILITY profiles and consolidate
previous OPERCMDS evidence in order to identify authorization patterns,
technical-account exceptions and candidates for future hardening.

Environment:

- IBM z/OS ADCD 1.11
- RACF
- TSO / ISPF
- SDSF
- Hercules laboratory environment

This is a controlled assessment lab.

No RACF authorization changes were performed.

## Scope

The review focused on:

- FACILITY profile inventory
- STGADMIN generic controls
- z/OS UNIX BPX privileged controls
- explicit technical-account permissions
- existing OPERCMDS evidence from Lab 12
- security classification using KEEP / REVIEW / HARDEN CANDIDATE

The objective was not to audit every FACILITY profile.

## Profiles reviewed

### Storage administration

- STGADMIN.*
- STGADMIN.IGD.*

### z/OS UNIX privileged functions

- BPX.SUPERUSER
- BPX.SERVER
- BPX.FILEATTR.APF
- BPX.FILEATTR.PROGCTL

## Key results

The reviewed STGADMIN profiles showed:

- UACC(NONE)
- WARNING(NO)
- auditing of failed READ attempts
- explicit privileged access

This provides a reasonable baseline for the reviewed storage administration
controls.

The reviewed BPX profiles also showed UACC(NONE), but several explicit
technical-account permissions require functional justification before they
can be considered least-privilege compliant.

No permissions were removed because the evidence collected does not establish
that those technical identities are unnecessary.

## OPERCMDS correlation

Lab 12 previously reviewed OPERCMDS, JES2, SDSF and console authority.

The searched OPERCMDS profile patterns did not return matching granular
profiles, while tested SDSF display commands returned operational output.

This does NOT prove unrestricted operator-command authority.

It identifies a hardening candidate requiring deeper effective-authority
analysis before RACF OPERCMDS profiles are introduced.

## Security engineering decision

The lab deliberately separates:

- observed configuration
- potential risk
- confirmed vulnerability
- future remediation

An explicit RACF permission is not automatically a vulnerability.

Production-style remediation requires functional ownership, impact analysis,
change control, validation and rollback planning.

## Final status

LAB 25: COMPLETED

No uncontrolled RACF changes performed.
