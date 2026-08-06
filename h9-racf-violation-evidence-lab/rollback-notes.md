# Rollback Notes

This lab primarily generated evidence and did not require destructive changes.

If the lab-specific access granted in earlier labs must be removed:

```text
PERMIT 'IBMUSER.SECLAB.GRANTED.*' CLASS(DATASET) ID(H7USER) DELETE
SETROPTS GENERIC(DATASET) REFRESH
```

Do not delete `H7USER` yet if continuing with later labs.

Do not change `SYS1.*`, `ADCD.Z111S.*`, STARTED, APF or OPERCMDS profiles as part of this rollback.
