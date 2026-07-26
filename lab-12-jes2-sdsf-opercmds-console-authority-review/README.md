# Lab 12 — JES2 / SDSF / OPERCMDS / Console Authority Review

## Purpose

This lab reviews the RACF and SDSF evidence related to JES2, spool visibility, operator command controls, and SDSF authority on an IBM ADCD z/OS 1.11 laboratory system.

The objective is not to harden the system yet. The objective is to document whether the environment shows granular RACF profiles for operational control classes such as `OPERCMDS`, `JESJOBS`, `JESSPOOL`, and `SDSF`, and whether basic display commands can be issued from SDSF.

## Scope

Reviewed areas:

- RACF `SETROPTS LIST` security class baseline.
- `OPERCMDS` profile searches for MVS, JES2 and JES `$` command controls.
- `JESJOBS` profile search.
- `JESSPOOL` profile searches for general spool, SYSLOG, MASTER and IBMUSER patterns.
- `SDSF` profile search.
- SDSF evidence for `/D A,L` and JES2 `$D SPOOL` display output.

## Key result

The lab evidence shows that the relevant RACF classes are present in the SETROPTS class lists, but the searched profile patterns returned no matching profiles. At the same time, SDSF accepted display commands and returned operational output.

In an ADCD training system this is expected and useful for learning. In a production review, the same pattern would require deeper analysis of how JES2, SDSF, spool access and operator commands are actually restricted.

## Evidence source

Original uploaded source document:

- `evidence/source-documents/LAB12.docx`

Extracted screenshots:

- `evidence/screenshots/`
