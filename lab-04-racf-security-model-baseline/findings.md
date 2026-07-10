# Findings

## Finding 1 - IBMUSER is highly privileged

Command:

```text
LISTUSER IBMUSER
```

Observed:

```text
USER=IBMUSER
DEFAULT-GROUP=SYS1
ATTRIBUTES=SPECIAL OPERATIONS
REVOKE DATE=NONE
LAST-ACCESS=26.191/09:18:40
```

Interpretation:

- SPECIAL is a powerful RACF administration attribute.
- OPERATIONS can provide broad operational access to many datasets.
- This is common in ADCD training systems, but it is not a least-privilege production model.

Security meaning:

IBMUSER is a lab superuser. Evidence and experiments should be clearly labelled as lab-only.

---

## Finding 2 - SYS1 is a central/powerful RACF group

Command:

```text
LISTGRP SYS1
```

Observed:

```text
GROUP=SYS1
OWNER=IBMUSER
SUPERIOR GROUP=NONE
USER=IBMUSER AUTH=JOIN
```

Interpretation:

- SYS1 is a top-level group in this RACF hierarchy.
- IBMUSER owns or administers key RACF structures in the ADCD lab.
- The group contains many subgroups related to system components and products.

Security meaning:

The group structure confirms this is an administrative training image, not a hardened production model.

---

## Finding 3 - DATASET class is active and generic profiles are supported

Command:

```text
SETROPTS LIST
```

Observed:

```text
ACTIVE CLASSES = DATASET ... STARTED ...
GENERIC PROFILE CLASSES = DATASET ... STARTED ...
```

Interpretation:

- RACF can protect datasets through the DATASET class.
- RACF can use generic profiles such as IBMUSER.* or ADCD.Z111S.*.
- STARTED class is active, allowing RACF control of started task identities.

Security meaning:

The controls exist, but actual protection depends on profiles being defined correctly.

---

## Finding 4 - STARTED class is RACLISTed

Command:

```text
SETROPTS LIST
```

Observed:

```text
SETR RACLIST CLASSES = ... STARTED ...
```

Interpretation:

- STARTED profiles are cached in memory for performance.
- Changes to STARTED profiles normally require:

```text
SETROPTS RACLIST(STARTED) REFRESH
```

Security meaning:

A profile change may not take effect until the RACLIST cache is refreshed.

---

## Finding 5 - PROTECT-ALL and ADSP are not active

Command:

```text
SETROPTS LIST
```

Observed:

```text
AUTOMATIC DATASET PROTECTION IS NOT IN EFFECT
PROTECT-ALL OPTION IS NOT IN EFFECT
```

Interpretation:

- RACF will not automatically protect all datasets through PROTECTALL.
- RACF will not automatically create dataset protection through ADSP.

Security meaning:

Unprofiled datasets can exist. This explains why individual sensitive libraries may appear unprotected in Health Checker.

---

## Finding 6 - Enhanced generic naming is not active

Command:

```text
SETROPTS LIST
```

Observed:

```text
ENHANCED GENERIC NAMING IS NOT IN EFFECT
```

Interpretation:

- The system uses older RACF generic naming behavior.
- Generic profile syntax must be treated carefully.

Security meaning:

Do not assume modern **-style generic matching behavior in this environment.

---

## Finding 7 - No RACF profile found for IBMUSER.*

Command:

```text
LISTDSD DA('IBMUSER.*') GENERIC ALL
```

Observed:

```text
ICH35003I NO RACF DESCRIPTION FOUND FOR IBMUSER.*
```

Interpretation:

- No generic DATASET profile was found for IBMUSER.*.

Security meaning:

In a production model, user datasets should have explicit or generic RACF protection. In this lab, IBMUSER's SPECIAL/OPERATIONS attributes can mask this weakness.

---

## Finding 8 - No RACF profile found for ADCD.Z111S.LINKLIB

Command:

```text
LISTDSD DA('ADCD.Z111S.LINKLIB') ALL
```

Observed:

```text
ICH35003I NO RACF DESCRIPTION FOUND FOR ADCD.Z111S.LINKLIB
```

Interpretation:

- No explicit RACF DATASET profile was found for this sensitive executable library.

Security meaning:

This aligns with the earlier Health Checker RACF_SENSITIVE_RESOURCES finding. It is a meaningful hardening candidate, but not something to remediate blindly on a running ADCD system.

---

## Finding 9 - STARTED profiles exist, but wildcard RLIST is too broad

Command:

```text
RLIST STARTED * ALL
```

Observed examples:

```text
CLASS STARTED NAME HZSPROC
OWNER=IBMUSER
UNIVERSAL ACCESS=NONE
YOUR ACCESS=ALTER
WARNING=NO
AUDITING=FAILURES(READ)
USER IBMUSER ACCESS ALTER
```

Also observed:

```text
CLASS STARTED NAME AOPSTART.* (G)
OWNER=LARRYWD
UNIVERSAL ACCESS=NONE
```

Interpretation:

- STARTED profiles exist.
- Wildcard RLIST produces a very long report.
- The provided screenshot does not show STDATA for HZSPROC, so further targeted investigation is needed before claiming which RACF user HZSPROC maps to.

Security meaning:

Started tasks are a critical security area. Future labs should inspect specific started task mappings such as HZSPROC, TCPIP, SDSF, JES2, DB2, CICS, SSHD, and FTPD.
