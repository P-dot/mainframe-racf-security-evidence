# Hardening Notes — Applying the H1 Lesson Safely

## Recommended Pattern

For sensitive datasets, prefer:

```text
ADDSD 'HLQ.SENSITIVE.*' UACC(NONE)
PERMIT 'HLQ.SENSITIVE.*' ID(required-user-or-group) ACCESS(required-level)
ALTDSD 'HLQ.SENSITIVE.*' GENERIC AUDIT(FAILURES(READ))
```

## Avoid Blind UACC(READ)

`UACC(READ)` should not be used casually. It is acceptable here only because the profile is a training object under `IBMUSER.SECLAB.*`.

## Verify With LISTDSD

After any profile change, verify with:

```text
LISTDSD DA('profile-name') ALL
```

Do not assume the control exists just because the command was typed.

## Production Change-Control Checklist

Before applying this to real resources:

1. Identify the exact dataset pattern.
2. Confirm current protection and current users.
3. Confirm business/application owners.
4. Plan access list entries.
5. Enable audit in a controlled way.
6. Test with non-production IDs.
7. Prepare rollback commands.
8. Document before/after screenshots.

## Never Apply This Blindly To

```text
SYS1.*
ADCD.Z111S.*
APF libraries
LINKLIST libraries
LPALST/APF controlled datasets
RACF database datasets
JES spool datasets
zFS root/system filesystems
```
