# Lab 04 - RACF Security Model Baseline

## Purpose

This lab documents a read-only RACF security baseline on an ADCD z/OS 1.11 learning environment.

The goal is to understand the mainframe security model before attempting any remediation:

- RACF user attributes
- RACF group membership
- global SETROPTS options
- DATASET class status
- generic dataset profile coverage
- STARTED class visibility
- relationship with previous Health Checker / RACF_SENSITIVE_RESOURCES findings

## Scope

This lab is intentionally read-only. It does not modify RACF profiles, datasets, PARMLIB, PROCLIB, APF, LINKLIST, or started tasks.

## Environment

- Platform: ADCD z/OS 1.11 learning system
- Security product: RACF
- User inspected: IBMUSER
- Primary group inspected: SYS1
- Evidence source: ISPF option 6 command shell screenshots

## Key result

The system is usable as a lab, but the baseline shows several security weaknesses that would be high-risk in production:

- IBMUSER has SPECIAL and OPERATIONS.
- IBMUSER is connected to powerful groups, including SYS1.
- DATASET class is active and supports generic profiles.
- PROTECT-ALL is not in effect.
- ADSP is not in effect.
- Enhanced generic naming is not in effect.
- No RACF dataset profile was found for IBMUSER.*.
- No RACF dataset profile was found for ADCD.Z111S.LINKLIB.
- STARTED class is active and RACLISTed.

## Evidence

Screenshots are stored under:

```text
evidence/screenshots/
```

## Safety note

Do not apply remediation such as SETROPTS PROTECTALL or ADDSD ADCD.Z111S.* UACC(NONE) directly to the live ADCD environment without a recovery plan. Those changes can break TSO, SDSF, VTAM, TCPIP, CICS, DB2, or IPL dependencies.
