# Lab 06 — Technical Users Privilege Review

## Purpose

This lab audits RACF technical user IDs used by important z/OS started tasks in the ADCD z/OS V1R11 laboratory environment.

The previous lab showed that several started tasks run under technical IDs such as `START1`, `START2`, `TCPIP`, `FTPD`, `WEBSRV`, and `OMVSKERN`. This lab checks the RACF user profiles and key group relationships behind those IDs.

## Scope

Read-only review of:

- `LISTUSER START1`
- `LISTUSER START2`
- `LISTUSER TCPIP`
- `LISTUSER FTPD`
- `LISTUSER WEBSRV`
- `LISTUSER OMVSKERN`
- `LISTUSER IBMUSER`
- `LISTGRP SYS1`
- selected `LISTGRP` validation commands

## Safety

No RACF changes were made. The following change commands were intentionally avoided:

- `ALTUSER`
- `ADDUSER`
- `CONNECT`
- `REMOVE`
- `PERMIT`
- `RDEFINE`
- `RALTER`
- `SETROPTS REFRESH`

## Main result

The lab confirms a typical ADCD training pattern:

- `IBMUSER` remains highly privileged with `SPECIAL` and `OPERATIONS`.
- `START1` is a protected technical ID but has `OPERATIONS`.
- `START2` is protected and has no global special attributes shown, but many critical tasks run under it.
- `TCPIP`, `FTPD`, `WEBSRV`, and `OMVSKERN` are service IDs with no global special attributes shown.
- Some service IDs are not shown as `PROTECTED`, which should be reviewed in a hardened environment.
- `SYS1` is the central RACF group and contains many system/service IDs.

## Evidence

Screenshots are stored in `evidence/screenshots/`.
The original Word file provided by the lab operator is stored in `evidence/source-documents/LIST.docx`.
