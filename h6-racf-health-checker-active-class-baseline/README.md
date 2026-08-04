# H6 — RACF Health Checker Hardening: UNIXPRIV / TEMPDSN / OPERCMDS Active-Class Baseline

## Objective

This lab improves IBM Health Checker RACF active-class metrics in a controlled z/OS ADCD/Hercules laboratory environment.

The scope is deliberately limited to the active-class baseline for:

- `RACF_UNIXPRIV_ACTIVE`
- `RACF_TEMPDSN_ACTIVE`
- `RACF_OPERCMDS_ACTIVE`

The lab does not perform mass dataset hardening and does not remediate broad high-risk checks such as `RACF_SENSITIVE_RESOURCES`, `RACF_IBMUSER_REVOKED`, `TAPEVOL`, `SYS1.*`, `ADCD.Z111S.*`, `STARTED`, APF, LINKLIST, CICS, DB2, or MQ.

## Environment

- Platform: z/OS ADCD 1.11 running on Hercules
- Security manager: RACF
- Evidence source: `RACF.docx`
- Primary interface: TSO / ISPF option 6 and SDSF LOG / CK
- Health Checker started task: `HZSPROC`

## Summary of Result

Initial evidence showed `HZSPROC NOT ACTIVE`, followed by a clean start of `HZSPROC`. After running the IBMRACF checks, `RACF_UNIXPRIV_ACTIVE` was already successful, while `RACF_TEMPDSN_ACTIVE` and `RACF_OPERCMDS_ACTIVE` were still exceptions.

The lab then verified existing `UNIXPRIV` profiles, activated `TEMPDSN`, configured `OPERCMDS` with a guarded laboratory profile, enabled the class, and reran Health Checker.

Final evidence shows:

| Check | Final Status |
|---|---|
| `RACF_UNIXPRIV_ACTIVE` | `SUCCESSFUL` |
| `RACF_TEMPDSN_ACTIVE` | `SUCCESSFUL` |
| `RACF_OPERCMDS_ACTIVE` | `SUCCESSFUL` |
| `RACF_SENSITIVE_RESOURCES` | Still exception / out of scope |

## Professional Value

This lab demonstrates controlled RACF hardening linked to IBM Health Checker evidence. It shows the difference between broad auditing and targeted remediation: only the active-class metrics are improved, while high-risk dataset and privileged-ID remediations are intentionally deferred.
