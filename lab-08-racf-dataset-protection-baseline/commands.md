# Commands Executed

All RACF commands were executed from ISPF option 6.

## ADCD system libraries

```text
LISTDSD DA('ADCD.Z111S.PARMLIB') ALL
LISTDSD DA('ADCD.Z111S.PROCLIB') ALL
LISTDSD DA('ADCD.Z111S.LINKLIB') ALL
LISTDSD DA('ADCD.Z111S.VTAMLIB') ALL
```

## SYS1 system libraries

```text
LISTDSD DA('SYS1.PARMLIB') ALL
LISTDSD DA('SYS1.PROCLIB') ALL
LISTDSD DA('SYS1.LINKLIB') ALL
LISTDSD DA('SYS1.LPALIB') ALL
LISTDSD DA('SYS1.NUCLEUS') ALL
```

## Generic DATASET profile searches

```text
SEARCH CLASS(DATASET) MASK(ADCD.Z111S.*)
SEARCH CLASS(DATASET) MASK(SYS1.*)
```

## OMVS filesystem display

Executed from SDSF as a console display command:

```text
/D OMVS,F
```

## Commands deliberately not executed

```text
ADDSD
PERMIT
RALTER
SETROPTS PROTECTALL
SETROPTS ADSP
SETROPTS GENERIC(DATASET) REFRESH
```
