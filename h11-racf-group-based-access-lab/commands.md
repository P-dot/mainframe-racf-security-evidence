# Commands — H11 RACF Group-Based Access Lab

## Baseline identity and group checks

```text
LISTUSER H7USER
LISTGRP H7GRP
```

## Create laboratory dataset

```text
ALLOC DATASET('IBMUSER.SECLAB.GROUP.DATA') NEW CATALOG UNIT(SYSDA) SPACE(1,1) TRACKS DSORG(PS) RECFM(F B) LRECL(80) BLKSIZE(800)
LISTCAT ENT('IBMUSER.SECLAB.GROUP.DATA') ALL
```

## Dataset content used

```text
GROUP TEST DATASET
This file is protected using a RACF group-based access model.
H7USER receives access through H7GRP, not through an individual PERMIT.
```

## Create closed RACF profile

```text
ADDSD 'IBMUSER.SECLAB.GROUP.*' UACC(NONE)
ALTDSD 'IBMUSER.SECLAB.GROUP.*' AUDIT(FAILURES(READ))
LISTDSD DA('IBMUSER.SECLAB.GROUP.*') ALL
```

## Grant access to the group, not the user

```text
PERMIT 'IBMUSER.SECLAB.GROUP.*' CLASS(DATASET) ID(H7GRP) ACCESS(READ)
SETROPTS GENERIC(DATASET) REFRESH
LISTDSD DA('IBMUSER.SECLAB.GROUP.*') ALL
```

## Remove group access

```text
PERMIT 'IBMUSER.SECLAB.GROUP.*' CLASS(DATASET) ID(H7GRP) DELETE
SETROPTS GENERIC(DATASET) REFRESH
LISTDSD DA('IBMUSER.SECLAB.GROUP.*') ALL
```

## Optional evidence search

```text
F H7USER
F ICH408I
```

