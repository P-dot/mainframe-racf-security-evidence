# Audit Scope

## In scope

The audit covered the following areas:

| Area | Scope |
|---|---|
| RACF global baseline | `SETROPTS LIST`, active classes, generic classes, RACLIST state, password/protection options |
| RACF privileged users | `IBMUSER`, `START1`, `START2`, `TCPIP`, `FTPD`, `WEBSRV`, `OMVSKERN` |
| Started tasks | STARTED class profiles and runtime owners observed in SDSF |
| OMVS identity | OMVS segment review, UID/GID exposure, UID(0), UNIXMAP |
| Sensitive datasets | `SYS1.*`, `ADCD.Z111S.*`, product datasets, zFS backing datasets |
| zFS | Mounted zFS filesystems and backing dataset protection |
| SAF general resources | `FACILITY`, `UNIXPRIV`, `SERVAUTH`, `STARTED` |
| JES/SDSF/operator authority | `OPERCMDS`, `JESJOBS`, `JESSPOOL`, `SDSF`, JES2 display evidence |
| Audit/accountability | RACF audit indicators, WARNING profiles, SMF display and `SMFPRM00` |
| Authorized code | APF list, PROGRAM class, LINKLIST, APF/LINKLIST dataset protection |

## Out of scope

The following items were not fully implemented in this audit:

- production RACF remediation;
- live changes to `SETROPTS`, `RDEFINE`, `PERMIT`, `RALTER`, `ALTUSER` or APF settings;
- zSecure Audit execution;
- DSMON report generation;
- IRRDBU00 unload analysis;
- SMF record decoding;
- production compliance certification;
- penetration testing against business workloads.

## Method

The audit used read-only commands wherever possible. Where commands failed, the failure was preserved as operational evidence rather than hidden or rewritten.

No system hardening changes were intentionally performed as part of this audit phase.
