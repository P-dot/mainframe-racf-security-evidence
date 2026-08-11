# Rollback Notes

If `USER2` must be restored to the original test state:

```text
PERMIT 'IBMUSER.SECLAB.GRANTED.*' CLASS(DATASET) ID(USER2) ACCESS(READ)
SETROPTS GENERIC(DATASET) REFRESH
LISTDSD DA('IBMUSER.SECLAB.GRANTED.*') ALL
```

If `H7GRP` must be restored on the group profile:

```text
PERMIT 'IBMUSER.SECLAB.GROUP.*' CLASS(DATASET) ID(H7GRP) ACCESS(READ)
SETROPTS GENERIC(DATASET) REFRESH
LISTDSD DA('IBMUSER.SECLAB.GROUP.*') ALL
```

Do not use rollback unless there is a clear reason.
