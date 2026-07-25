# Risk Analysis

## Risk pattern

The combined risk across Labs 07-11 is:

```text
Privileged users and technical IDs
+ UID(0) concentration
+ sensitive datasets/zFS without visible RACF DATASET profiles
+ limited evidence of granular SAF controls
+ SMF SYS1.MAN recording not being used
```

## Impact

In a production environment, this pattern would weaken:

- accountability
- forensic reconstruction
- change traceability
- privileged access monitoring
- evidence required for audit/compliance

## Positive controls observed

- `STARTED` class is active and RACLISTed.
- `TCPIP.*`, `FTPD.*`, and `HZSPROC` STARTED profiles exist.
- These profiles show `UACC(NONE)`.
- These profiles show `WARNING(NO)`.
- These profiles show `AUDITING FAILURES(READ)`.

## Weaknesses observed

- Several hardening-related RACF global options are not enabled.
- Search syntax for `AUDIT` did not provide a clean discovery method in this environment.
- SMF display indicates SYS1.MAN recording is not being used.
- SMFPRM00 shows `NOACTIVE`.
- Auditing evidence is stronger for failures than for successful access to sensitive resources.

## Production recommendation

For a real production review, the next actions would be planned changes, not immediate lab commands:

- confirm active SMF recording method and destinations
- validate RACF SMF record collection
- enable appropriate SMF record types where required
- define or review audit options on critical profiles
- consider UAUDIT for selected privileged IDs
- review WARNING profiles regularly
- review STARTED, OPERCMDS, FACILITY, SERVAUTH, UNIXPRIV, and DATASET audit coverage
- validate evidence using reporting tools such as RACF reports, IRRDBU00, DSMON, or zSecure where available
