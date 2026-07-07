# Commands used in Lab 03

## Dataset allocation

Dataset created manually through ISPF 3.2:

```text
'SYS1.ADCD.HZSPDATA'
```

Recommended attributes:

```text
Volume serial      SBSYS1
Device type        SYSDA
Space units        TRACKS
Primary quantity   1
Secondary quantity 1
Directory blocks   0
Record format      FB
Record length      4096
Block size         4096
Data set name type blank/basic
```

## HZSPROC

Created in:

```text
ADCD.Z111S.PROCLIB(HZSPROC)
```

Working content:

```jcl
//HZSPROC PROC HZSPRM='00'
//HZSSTEP EXEC PGM=HZSINIT,REGION=0K,TIME=NOLIMIT,
//             PARM='SET PARMLIB=&HZSPRM'
//HZSPDATA DD DISP=OLD,DSN=SYS1.ADCD.HZSPDATA
```

## RACF definitions

Run from ISPF option 6:

```text
RDEFINE XFACILIT HZS.* UACC(READ)
SETROPTS RACLIST(XFACILIT) REFRESH
RDEFINE STARTED HZSPROC STDATA(USER(START1) GROUP(SYS1) TRUSTED(YES))
SETROPTS RACLIST(STARTED) REFRESH
```

## SDSF CK authorization

Edited:

```text
ADCD.Z111S.PARMLIB(ISFPRM00)
```

Added `CK` to SDSF command authorization list, for example:

```text
SO,NO,PUN,RDR,JC,SE,CK,RES),
```

Restart SDSF:

```text
/P SDSF
/S SDSF
```

## Start Health Checker

From SDSF or MVS console:

```text
/S HZSPROC
```

Check active tasks:

```text
DA
```

Open Health Checker:

```text
CK
```

Filter RACF checks:

```text
PREFIX=RACF*
```

Open:

```text
RACF_SENSITIVE_RESOURCES
```

## RACF investigation commands

```text
LISTDSD DA('ADCD.Z111S.LINKLIB') ALL
LISTDSD DA('ADCD.Z111S.VTAMLIB') ALL
LISTDSD DA('CBC.SCLBDLL') ALL
LISTDSD DA('ADCD.Z111S.*') GENERIC ALL
SETROPTS LIST
```

## Important errors encountered and fixes

### Wrong dummy program name

Wrong:

```jcl
//STEP1 EXEC PGM=IEBR14
```

Correct:

```jcl
//STEP1 EXEC PGM=IEFBR14
```

### Wrong Health Checker DD name

Wrong:

```jcl
//HZSDATA DD DISP=OLD,DSN=SYS1.ADCD.HZSPDATA
```

Correct:

```jcl
//HZSPDATA DD DISP=OLD,DSN=SYS1.ADCD.HZSPDATA
```

### SDSF CK not authorized

Fix: add `CK` to SDSF authorization list in `ISFPRM00` and restart SDSF.
