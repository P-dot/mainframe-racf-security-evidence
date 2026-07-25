# RACF Audit and Accountability Concepts Applied

## WARNING

A RACF profile in WARNING mode can allow access while issuing a warning/audit signal. In this lab, searches for WARNING profiles in DATASET, FACILITY, STARTED, and SERVAUTH returned no entries.

## AUDITING

The direct STARTED profile reviews show:

```text
AUDITING
FAILURES(READ)
```

This means failed READ-level access attempts against those profiles are audited. It is useful but limited: it is not the same as auditing every successful use of a sensitive privilege.

## UACC(NONE)

The reviewed STARTED profiles show `UACC(NONE)`, which is a strong default posture for protected resources because unspecified users do not receive access through universal access.

## Accountability limitation

The main limitation is not the absence of all audit settings. The main limitation is that SMF SYS1.MAN recording is shown as not being used. Audit settings are only operationally useful if the relevant event records are collected, retained, and reviewable.
