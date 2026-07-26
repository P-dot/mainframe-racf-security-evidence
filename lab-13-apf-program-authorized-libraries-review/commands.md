# Commands Used

## RACF PROGRAM review

```text
SEARCH CLASS(PROGRAM) MASK(*)
```

Purpose: identify visible profiles in the RACF `PROGRAM` class.

Captured result: profiles were listed, including several `BPX*` entries and `RLOGIND`.

## APF display

Executed from SDSF LOG:

```text
/D PROG,APF
```

Purpose: display the active APF-authorized library list.

## LINKLIST display

Executed from SDSF LOG:

```text
/D PROG,LNKLST
```

Purpose: display active LINKLIST libraries.

## LPA display attempt

Executed from SDSF LOG:

```text
/D PROG,LPA
```

Captured result: syntax error was returned. No LPA library list was captured in this lab.

## Dataset profile checks

Executed from ISPF option 6:

```text
LISTDSD DA('SYS1.LNKLIB') ALL
LISTDSD DA('SYS1.SVCLIB') ALL
LISTDSD DA('SYS1.LPALIB') ALL
LISTDSD DA('ADCD.Z111S.LINKLIB') ALL
LISTDSD DA('CEE.SCLBDLL') ALL
LISTDSD DA('CEE.SCEERUN') ALL
```

Notes:

- `SYS1.LNKLIB` appears to be a typo for `SYS1.LINKLIB`; it is preserved as captured evidence.
- `CEE.SCLBDLL` was captured; the originally planned `CBC.SCLBDLL` check was not captured.

## Generic dataset profile search

```text
SEARCH CLASS(DATASET) MASK(SYS1.*)
```

Purpose: look for visible generic RACF `DATASET` profiles covering `SYS1.*`.
