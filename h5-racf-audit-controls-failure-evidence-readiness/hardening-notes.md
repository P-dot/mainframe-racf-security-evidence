# Hardening Notes

## What this lab hardened

The lab hardened only the sandbox profile:

```text
IBMUSER.SECLAB.AUDIT.*
```

Final state:

```text
UACC(NONE)
WARNING(NO)
AUDIT(FAILURES(READ))
```

## Production guidance

For real datasets:

1. Define explicit profiles.
2. Use `UACC(NONE)` for sensitive resources.
3. Grant only required access through access lists.
4. Configure targeted auditing for sensitive data.
5. Verify SMF is active and dump/processing procedures are healthy.
6. Test with a non-privileged user, not with a global administrator.

## Do not apply blindly

Do not copy these commands to `SYS1.*`, `ADCD.Z111S.*`, APF libraries, DB2 datasets, CICS datasets, or production HLQs without change control and rollback.
