# Lab 03 Findings

## Finding 1 - Health Checker was not configured initially

### Evidence

- SDSF `CK` was initially not authorized.
- `HZSPROC` was not active.
- `/S HZSPROC` initially failed because the procedure was not in an active PROCLIB.

### Resolution

- Created `SYS1.ADCD.HZSPDATA`.
- Created `ADCD.Z111S.PROCLIB(HZSPROC)`.
- Updated RACF `XFACILIT` and `STARTED` definitions.
- Added `CK` authorization to `ADCD.Z111S.PARMLIB(ISFPRM00)`.
- Restarted SDSF.
- Started `HZSPROC`.

## Finding 2 - RACF_SENSITIVE_RESOURCES reported HIGH APF exceptions

### Evidence

The check displayed:

```text
CHECK(IBMRCF,RACF_SENSITIVE_RESOURCES)
CHECK SEVERITY: HIGH
APF Dataset Report
```

Several APF datasets appeared with `E` status.

### Interpretation

`E` indicates an exception. In this case, the APF libraries are sensitive system resources and require adequate RACF DATASET protection.

## Finding 3 - Missing RACF DATASET profiles

### Evidence

Commands such as:

```text
LISTDSD DA('ADCD.Z111S.LINKLIB') ALL
LISTDSD DA('ADCD.Z111S.*') GENERIC ALL
```

returned:

```text
NO RACF DESCRIPTION FOUND
```

### Interpretation

The APF libraries reviewed did not have matching discrete or generic RACF DATASET profiles.

## Finding 4 - Global RACF options increase relevance of missing profiles

### Evidence

`SETROPTS LIST` showed:

```text
PROTECT-ALL OPTION IS NOT IN EFFECT
AUTOMATIC DATASET PROTECTION IS NOT IN EFFECT
ENHANCED GENERIC NAMING IS NOT IN EFFECT
```

### Interpretation

Because `PROTECTALL` and `ADSP` are not active, missing DATASET profiles are especially relevant in this lab environment.

## Risk statement

APF libraries can contain authorized programs. Missing or weak RACF protection over APF datasets can create privilege-escalation exposure if unauthorized users obtain update access.

## Recommended next step

Do not apply broad profiles to real APF libraries without full access analysis. First perform a controlled remediation lab using a test dataset.
