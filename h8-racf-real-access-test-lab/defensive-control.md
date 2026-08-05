# Defensive Control

The defensive pattern proven by this lab is:

```text
UACC(NONE)
+ explicit access list
+ audit on sensitive failure paths
+ validation with a non-privileged test identity
```

## Why this matters

A RACF profile that looks correct from an administrator session must still be validated from a realistic user session.

This prevents false confidence caused by testing only with `IBMUSER`, which has elevated authority in this lab environment.
