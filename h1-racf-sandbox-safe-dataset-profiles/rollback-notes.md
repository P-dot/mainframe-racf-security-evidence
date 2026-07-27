# Rollback Notes — H1 Sandbox

The sandbox was intentionally left in place for later labs.

Do not run rollback unless you want to remove the H1 training environment.

## Remove Access List Entry

```text
PERMIT 'IBMUSER.SECLAB.GRANTED.*' CLASS(DATASET) ID(USER2) DELETE
```

## Delete RACF Dataset Profiles

```text
DELDSD 'IBMUSER.SECLAB.PUBLIC.*'
DELDSD 'IBMUSER.SECLAB.PRIVATE.*'
DELDSD 'IBMUSER.SECLAB.GRANTED.*'
```

## Delete Lab Datasets

Only after profiles are no longer needed:

```text
DELETE 'IBMUSER.SECLAB.PUBLIC.DATA'
DELETE 'IBMUSER.SECLAB.PRIVATE.DATA'
DELETE 'IBMUSER.SECLAB.GRANTED.DATA'
```

## Verification After Rollback

```text
LISTCAT ENT('IBMUSER.SECLAB.PUBLIC.DATA') ALL
LISTCAT ENT('IBMUSER.SECLAB.PRIVATE.DATA') ALL
LISTCAT ENT('IBMUSER.SECLAB.GRANTED.DATA') ALL

LISTDSD DA('IBMUSER.SECLAB.PUBLIC.*') ALL
LISTDSD DA('IBMUSER.SECLAB.PRIVATE.*') ALL
LISTDSD DA('IBMUSER.SECLAB.GRANTED.*') ALL
```

Expected result after complete rollback: datasets and profiles are no longer found.
