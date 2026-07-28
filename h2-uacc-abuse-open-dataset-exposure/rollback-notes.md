# Rollback Notes

H2 mainly reviews and documents the existing H1 sandbox state. It also adds benign text to sandbox datasets.

## To remove only dataset content

Edit the test datasets and delete the sample lines.

## To revert the vulnerable profile in a later remediation lab

If a later lab changes `PUBLIC` to `UACC(NONE)`, the vulnerable demo state can be restored with:

```text
ALTDSD 'IBMUSER.SECLAB.PUBLIC.*' UACC(READ)
SETROPTS GENERIC(DATASET) REFRESH
```

## To remove the sandbox completely

Do not do this until the full RACF hardening sandbox series is finished.

Possible cleanup later:

```text
PERMIT 'IBMUSER.SECLAB.GRANTED.*' CLASS(DATASET) ID(USER2) DELETE
DELDSD 'IBMUSER.SECLAB.PUBLIC.*'
DELDSD 'IBMUSER.SECLAB.PRIVATE.*'
DELDSD 'IBMUSER.SECLAB.GRANTED.*'
SETROPTS GENERIC(DATASET) REFRESH
```

Dataset deletion would be handled separately and only after confirming no later lab depends on them.
