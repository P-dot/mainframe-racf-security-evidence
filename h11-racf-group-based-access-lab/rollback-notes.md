# Rollback Notes

To remove group access:

```text
PERMIT 'IBMUSER.SECLAB.GROUP.*' CLASS(DATASET) ID(H7GRP) DELETE
SETROPTS GENERIC(DATASET) REFRESH
```

To verify rollback:

```text
LISTDSD DA('IBMUSER.SECLAB.GROUP.*') ALL
```

Expected final state:

```text
UACC(NONE)
No H7GRP in standard access list
No H7USER in standard access list
```

Do not delete `H7USER` or `H7GRP`; they are reused by later labs.

