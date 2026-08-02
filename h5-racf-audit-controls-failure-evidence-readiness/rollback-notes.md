# Rollback Notes

The lab final state is intentionally safe. If rollback is required, use only the sandbox profile.

## Return to no audit

```text
ALTDSD 'IBMUSER.SECLAB.AUDIT.*' NOAUDIT
SETROPTS GENERIC(DATASET) REFRESH
LISTDSD DA('IBMUSER.SECLAB.AUDIT.*') ALL
```

## Delete the sandbox profile only if intentionally cleaning up

```text
DELDSD 'IBMUSER.SECLAB.AUDIT.*'
SETROPTS GENERIC(DATASET) REFRESH
```

Do not delete or modify any system dataset profile as part of this rollback.
