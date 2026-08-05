# Commands

## Baseline checks

```text
LISTGRP H7GRP
LISTUSER H7USER
LISTUSER IBMUSER TSO
```

## Create the lab group

```text
ADDGROUP H7GRP SUPGROUP(SYS1) OWNER(IBMUSER)
LISTGRP H7GRP
```

## Create the lab user

```text
ADDUSER H7USER DFLTGRP(H7GRP) OWNER(IBMUSER) NAME('H7 RACF TEST USER') PASSWORD(<temporary-password>)
LISTUSER H7USER
```

## Add TSO segment

The working TSO pattern was taken from `IBMUSER` and adapted to `H7USER`.

```text
ALTUSER H7USER TSO(ACCTNUM(ACCT#) PROC(DBSPROC9))
ALTUSER H7USER TSO(UNIT(3390) COMMAND(ISPF))
ALTUSER H7USER TSO(SIZE(2048000) MAXSIZE(2096128))
LISTUSER H7USER TSO
```

## Authorize TSO account number

The first logon attempt reached TSO but failed because the account number was not authorized for the user. The fix was to permit `H7USER` to use the existing `ACCT#` account number.

```text
RLIST ACCTNUM ACCT# ALL
PERMIT ACCT# CLASS(ACCTNUM) ID(H7USER) ACCESS(READ)
SETROPTS RACLIST(ACCTNUM) REFRESH
ALTUSER H7USER TSO(ACCTNUM(ACCT#))
LISTUSER H7USER TSO
```

## Permit sandbox dataset read access

```text
PERMIT 'IBMUSER.SECLAB.GRANTED.*' CLASS(DATASET) ID(H7USER) ACCESS(READ)
SETROPTS GENERIC(DATASET) REFRESH
LISTDSD DA('IBMUSER.SECLAB.GRANTED.*') ALL
```
