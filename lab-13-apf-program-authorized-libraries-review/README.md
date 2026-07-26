# Lab 13 — APF / PROGRAM / Authorized Libraries Review

## Purpose

This lab documents a read-only review of APF-authorized libraries, LINKLIST entries, RACF `PROGRAM` profiles, and RACF `DATASET` protection evidence for selected authorized-code libraries in an IBM ADCD z/OS lab environment.

The lab continues the RACF security evidence series:

- Previous labs reviewed privileged IDs, OMVS UID(0), zFS backing datasets, SAF classes, SMF/audit, and JES/SDSF/OPERCMDS.
- This lab focuses on authorized code paths: APF, LINKLIST, `PROGRAM`, and the dataset protection posture around sensitive load libraries.

## Scope

Evidence captured:

- `SEARCH CLASS(PROGRAM) MASK(*)`
- `/D PROG,APF`
- `/D PROG,LNKLST`
- Attempted `/D PROG,LPA` display
- `LISTDSD` checks for selected sensitive libraries
- `SEARCH CLASS(DATASET) MASK(SYS1.*)`

No RACF or system configuration changes were performed.

## Lab environment

- Platform: IBM ADCD z/OS lab system
- Security manager: RACF
- User context: IBMUSER / TSO / ISPF / SDSF
- Evidence source: `LAB13.docx`

## Important evidence quality note

Some commands shown in the command history are repeated from earlier labs. They are not treated as new Lab 13 findings unless directly related to APF, PROGRAM, LINKLIST, or authorized library protection.

There are also two useful caveats:

- The current Lab 13 evidence contains `LISTDSD DA('SYS1.LNKLIB') ALL`, which appears to be a typo for `SYS1.LINKLIB`. The typo result is preserved as evidence but is not treated as proof about `SYS1.LINKLIB` in this lab.
- The current Lab 13 evidence contains `LISTDSD DA('CEE.SCLBDLL') ALL`; the originally planned check was `CBC.SCLBDLL`. The captured result is preserved as-is.

## Conclusion

The lab shows that APF and LINKLIST structures are active and populated, that RACF `PROGRAM` profiles exist, and that several selected libraries reviewed in the captured evidence do not show visible RACF `DATASET` profiles. In a production system, this area would require strict dataset protection and formal change control because APF-authorized libraries and LINKLIST libraries are sensitive code execution paths.
