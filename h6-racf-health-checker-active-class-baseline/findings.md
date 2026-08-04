# Findings

## Finding 1 — HZSPROC was initially inactive

The initial system state showed:

```text
IEE341I HZSPROC NOT ACTIVE
```

This was corrected by starting `HZSPROC` and verifying activity with `D A,HZSPROC`.

## Finding 2 — UNIXPRIV baseline was already hardened

The evidence shows `RACF_UNIXPRIV_ACTIVE` successful and `UNIXPRIV` profiles such as `SUPERUSER.FILESYS` and `SUPERUSER.FILESYS.CHOWN` defined with `UACC(NONE)`.

## Finding 3 — TEMPDSN required activation

`RACF_TEMPDSN_ACTIVE` appeared as an exception before activation. After `SETROPTS CLASSACT(TEMPDSN)`, Health Checker reported the check as successful.

## Finding 4 — OPERCMDS required careful activation

`RACF_OPERCMDS_ACTIVE` appeared as an exception. The lab identified existing OPERCMDS profiles and used a guarded `MVS.**` profile with `WARNING` mode before enabling and RACLISTing the class.

Final Health Checker evidence shows `RACF_OPERCMDS_ACTIVE` as successful.

## Finding 5 — Sensitive resources remain out of scope

`RACF_SENSITIVE_RESOURCES` remains an exception. This is expected and intentionally not remediated in this lab, because broad remediation of APF, LINKLIST, PARMLIB, RACF database, and sensitive datasets requires a separate plan with backup and rollback.
