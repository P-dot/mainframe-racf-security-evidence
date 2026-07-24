# Next steps

Recommended next lab:

```text
Lab 11 — RACF audit evidence and logging baseline
```

That lab should review:

```text
SETROPTS AUDIT options
AUDITING fields on DATASET / FACILITY / SERVAUTH / STARTED profiles
SMF availability and RACF logging assumptions
SDSF / Health Checker evidence
Which events would be visible to an auditor
```

Before remediation, also collect:

```text
RLIST CDT FACILITY ALL
RLIST CDT UNIXPRIV ALL
RLIST CDT SERVAUTH ALL
SEARCH CLASS(DATASET) MASK(DFH410*)
SEARCH CLASS(DATASET) MASK(DSN910*)
LISTCAT ENT('DFH410.ZFS') ALL
LISTCAT ENT('DSN910.SJVAZFS') ALL
LISTCAT ENT('DSN910.SDSNGLS') ALL
```

Do not harden ADCD system datasets blindly. Changes to `SYS1.*`, `ADCD.Z111S.*`, zFS backing datasets, or SAF classes can break boot, CICS, DB2, TCP/IP, SSH, or OMVS.
