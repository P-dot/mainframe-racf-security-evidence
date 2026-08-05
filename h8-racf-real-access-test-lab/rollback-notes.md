# Rollback Notes

This lab does not require destructive rollback.

If the H7USER permit must be removed from the GRANTED sandbox profile:

```text
PERMIT 'IBMUSER.SECLAB.GRANTED.*' CLASS(DATASET) ID(H7USER) DELETE
SETROPTS GENERIC(DATASET) REFRESH
```

If the ACCTNUM permit must be removed:

```text
PERMIT ACCT# CLASS(ACCTNUM) ID(H7USER) DELETE
SETROPTS RACLIST(ACCTNUM) REFRESH
```

Do not delete `H7USER` yet. It is needed for the next labs.
