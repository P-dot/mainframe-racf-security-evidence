# Hardening Notes

## Do not remediate in H2

The vulnerable profile is intentionally left unchanged so the next lab can demonstrate controlled remediation.

## Future remediation pattern

The safe fix for the public profile would be:

```text
ALTDSD 'IBMUSER.SECLAB.PUBLIC.*' UACC(NONE)
```

Then grant only required access:

```text
PERMIT 'IBMUSER.SECLAB.PUBLIC.*' CLASS(DATASET) ID(USER2) ACCESS(READ)
```

Then refresh generic dataset profiles if required:

```text
SETROPTS GENERIC(DATASET) REFRESH
```

## Production rule

Never blindly change UACC on production profiles without:

- impact analysis
- owner approval
- access-list design
- SMF/RACF logging validation
- rollback plan
- staged testing
