# Lab 03 - RACF Health Checker: RACF_SENSITIVE_RESOURCES

## Objective

Enable IBM Health Checker for z/OS in an ADCD z/OS 1.11 lab environment and use SDSF `CK` to review the RACF health check `RACF_SENSITIVE_RESOURCES`.

This lab demonstrates a real security-audit workflow:

1. Configure Health Checker runtime data set.
2. Create and start the `HZSPROC` started task.
3. Authorize SDSF `CK` access.
4. Open `RACF_SENSITIVE_RESOURCES`.
5. Interpret APF dataset exceptions.
6. Validate findings with RACF `LISTDSD` and `SETROPTS LIST`.

## Environment

- Platform: ADCD z/OS 1.11 lab system
- User: IBMUSER
- Tools: TSO/ISPF, SDSF, RACF commands, IBM Health Checker for z/OS
- Key SDSF panel: `CK`
- Key started task: `HZSPROC`

## Final result

The lab successfully reached:

```text
SDSF -> CK -> RACF_SENSITIVE_RESOURCES
```

The check displayed a `HIGH` severity report and APF dataset exceptions.

## Main findings

### 1. Health Checker was not initially active

Initial symptoms:

```text
CK COMMAND NOT AUTHORIZED
HC NOT ACTIVE
PROCEDURE HZSPROC WAS NOT FOUND
```

The issue was solved by:

- Creating `SYS1.ADCD.HZSPDATA`.
- Creating `HZSPROC` in active PROCLIB `ADCD.Z111S.PROCLIB`.
- Adding `CK` authorization in `ADCD.Z111S.PARMLIB(ISFPRM00)`.
- Starting `HZSPROC`.

### 2. Active PROCLIB found

The active PROCLIB was identified by locating an existing started task procedure:

```text
ADCD.Z111S.PROCLIB(SDSF)
```

Therefore, `HZSPROC` was created in:

```text
ADCD.Z111S.PROCLIB(HZSPROC)
```

### 3. Correct HZSPROC member

Final working PROC:

```jcl
//HZSPROC PROC HZSPRM='00'
//HZSSTEP EXEC PGM=HZSINIT,REGION=0K,TIME=NOLIMIT,
//             PARM='SET PARMLIB=&HZSPRM'
//HZSPDATA DD DISP=OLD,DSN=SYS1.ADCD.HZSPDATA
```

Important correction:

```text
DD name must be HZSPDATA, not HZSDATA.
```

### 4. SDSF CK authorization

SDSF used parameter suffix `00`, so the active parameter member was:

```text
ADCD.Z111S.PARMLIB(ISFPRM00)
```

The `CK` option was added to the command authorization list, for example:

```text
SO,NO,PUN,RDR,JC,SE,CK,RES),
```

After restarting SDSF, the `CK` panel became available.

### 5. RACF started task and XFACILIT definitions

RACF definitions used:

```text
RDEFINE XFACILIT HZS.* UACC(READ)
SETROPTS RACLIST(XFACILIT) REFRESH
RDEFINE STARTED HZSPROC STDATA(USER(START1) GROUP(SYS1) TRUSTED(YES))
SETROPTS RACLIST(STARTED) REFRESH
```

The system assigned the started task to user `START2` in the observed run, but the important result is that `HZSPROC` expanded from the active PROCLIB and started correctly.

### 6. RACF_SENSITIVE_RESOURCES result

The following check was reached:

```text
CHECK(IBMRCF,RACF_SENSITIVE_RESOURCES)
CHECK SEVERITY: HIGH
APF Dataset Report
```

Multiple APF libraries appeared with status:

```text
E = Exception
```

Examples observed:

```text
ADCD.Z111S.LINKLIB
ADCD.Z111S.VTAMLIB
CBC.SCLBDLL
CEE.SCEERUN
CSF.SCSFMOD0
CSQ700.SCSQAUTH
DFH410.CICS.SDFHAUTH
DSN910.SDSNLOAD
EQA410.SEQAAUTH
```

### 7. RACF verification

The following RACF commands were used to investigate the exceptions:

```text
LISTDSD DA('ADCD.Z111S.LINKLIB') ALL
LISTDSD DA('ADCD.Z111S.VTAMLIB') ALL
LISTDSD DA('CBC.SCLBDLL') ALL
LISTDSD DA('ADCD.Z111S.*') GENERIC ALL
```

Observed result:

```text
NO RACF DESCRIPTION FOUND
```

This indicates that RACF did not find an explicit or generic DATASET profile protecting those sensitive APF libraries.

### 8. SETROPTS global options

`SETROPTS LIST` showed:

```text
DATASET class is active
Generic DATASET profiles are active
Enhanced generic naming is not in effect
Protect-all option is not in effect
Automatic dataset protection is not in effect
```

Security interpretation:

```text
Sensitive APF libraries appeared in RACF_SENSITIVE_RESOURCES because they lacked adequate RACF DATASET profile protection in a system where PROTECTALL and ADSP were not active.
```

## Security interpretation

APF libraries are sensitive because they can contain authorized programs. If an unauthorized user can modify an APF library, this can create a major privilege-escalation risk.

In this ADCD lab, the finding should be treated as an audit result, not as an instruction to immediately restrict production-equivalent datasets. Directly applying `UACC(NONE)` to broad profiles such as `ADCD.Z111S.*` without a full access analysis could break z/OS components such as TSO, SDSF, VTAM, DB2, CICS or started tasks.

## Safe remediation approach

For the next lab, remediation should be tested on a controlled dataset first:

1. Create a test dataset.
2. Confirm no RACF profile exists.
3. Define a DATASET profile.
4. Set restrictive `UACC`.
5. Permit only required users/groups.
6. Validate using `LISTDSD`.
7. Only later design a controlled remediation plan for real APF libraries.

## Evidence

Screenshots are stored under:

```text
evidence/screenshots/
```

Recommended evidence to attach:

- SDSF CK panel showing Health Checker active.
- `RACF_SENSITIVE_RESOURCES` APF Dataset Report.
- `SETROPTS LIST` screenshots.
- SDSF/PROCLIB screenshots showing `ADCD.Z111S.PROCLIB`.
- SDSF parameter screenshot showing `ISFPRM00` authorization.
