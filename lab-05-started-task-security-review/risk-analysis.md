# Risk Analysis

## Risk 1 - Generic technical users

Several critical services run as `START1` or `START2`.

Examples:

```text
SDSF
CICSA
DB9GMSTR
DB9DIST
DB9IRLM
CSQ7MSTR
SSHD4
TN3270
PORTMAP
```

Security concern:

- Multiple subsystems share the same technical identity.
- Accountability is weaker.
- Least privilege is harder to prove.
- Compromise or misuse of one identity can affect many services.

Production recommendation:

- Define service-specific users where practical.
- Assign minimum required RACF permissions.
- Document each started task identity.
- Avoid broad generic users for unrelated services.

## Risk 2 - STARTED profiles without visible STDATA

Profiles such as `HZSPROC`, `FTPD.*`, and `TCPIP.*` exist but the captured outputs do not show `STDATA`.

Security concern:

- The identity assignment source is not fully evident from the captured profile.
- The runtime owner may come from ICHRIN03, default mapping, or another mechanism.

Production recommendation:

- Confirm the exact mapping source for each started task.
- Prefer explicit, documented STARTED class profiles with `STDATA(USER(...) GROUP(...))` where appropriate.
- Review `TRUSTED` and `PRIVILEGED` usage carefully.

## Risk 3 - Missing targeted profiles for active services

Searches did not find profiles for active services such as SDSF, SSHD4, CICS and DB2 naming patterns.

Security concern:

- Active critical services may not have easily identifiable STARTED class profiles.
- This weakens auditability and maintainability.

Production recommendation:

- Build a started task matrix.
- Match SDSF runtime owners against RACF profiles and parmlib/proclib definitions.
- Document exceptions.

## Lab conclusion

The environment is suitable for training and controlled research, but it is not a hardened production-style RACF configuration. The key professional takeaway is the method: compare RACF profile definitions, runtime owners, and system log evidence before making changes.
