# Lab 11 - RACF Audit, Logging and Accountability Baseline

## Purpose

This lab documents the audit and accountability baseline of the ADCD z/OS 1.11 RACF environment. Previous labs identified privileged technical identities, UID(0) usage, zFS filesystems mounted in read/write mode, and sensitive datasets without visible RACF DATASET profiles. This lab asks the next professional question: if privileged access is used, is there enough logging and audit evidence to investigate it?

## Scope

The lab reviews read-only evidence from:

- `SETROPTS LIST`
- RACF `SEARCH ... WARNING`
- RACF `SEARCH ... AUDIT`
- `RLIST STARTED TCPIP.* ALL`
- `RLIST STARTED FTPD.* ALL`
- `RLIST STARTED HZSPROC ALL`
- `/D SMF`
- `/D SMF,O`
- browse of `ADCD.Z111S.PARMLIB(SMFPRM00)`

No RACF, SMF, SETROPTS, or PARMLIB changes were made.

## High-level conclusion

The system shows useful RACF configuration and some STARTED profile auditing, but the SMF evidence indicates that SYS1.MAN recording is not being used and `SMFPRM00` contains `NOACTIVE`. The environment is therefore suitable as an ADCD training baseline, but not as a hardened production audit model.

## Evidence source

Original evidence document: `evidence/source-documents/lab11.docx`.

Screenshots are stored under `evidence/screenshots/`.
