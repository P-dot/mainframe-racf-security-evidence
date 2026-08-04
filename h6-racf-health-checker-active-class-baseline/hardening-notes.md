# Hardening Notes

## What was improved

- `HZSPROC` was started and used for RACF Health Checker validation.
- `UNIXPRIV` status remained successful.
- `TEMPDSN` was activated.
- `OPERCMDS` was activated and RACLISTed.
- Health Checker confirmed success for the three target active-class checks.

## What was not changed

- No `SYS1.*` dataset hardening.
- No `ADCD.Z111S.*` dataset hardening.
- No `STARTED` profile changes.
- No APF/LINKLIST remediation.
- No DB2, CICS, or MQ changes.
- No `RACF_IBMUSER_REVOKED` remediation.
- No `RACF_SENSITIVE_RESOURCES` remediation.

## Production note

`MVS.** UACC(NONE) WARNING` is a lab transition profile, not a final production design. A production policy should define granular profiles for command families and operator roles.
