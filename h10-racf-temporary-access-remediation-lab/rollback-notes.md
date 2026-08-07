# Rollback notes

The rollback for this lab is the removal of the temporary access-list entry:

```text
PERMIT 'IBMUSER.SECLAB.PRIVATE.*' CLASS(DATASET) ID(H7USER) DELETE
SETROPTS GENERIC(DATASET) REFRESH
LISTDSD DA('IBMUSER.SECLAB.PRIVATE.*') ALL
```

Expected final condition:

```text
UACC(NONE)
H7USER not present in the access list
H7USER browse attempt fails again
```

Do not delete the dataset or the RACF profile as part of this lab.
