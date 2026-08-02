# H5 — RACF AUDIT Controls & Failure Evidence Readiness

## Objective

This lab demonstrates how to configure RACF audit controls on a safe dataset profile and how to interpret the difference between dataset protection, RACF audit settings, and system-level logging readiness.

The lab uses only the sandbox namespace:

```text
IBMUSER.SECLAB.AUDIT.*
```

No production datasets, system libraries, APF libraries, DB2, CICS, STARTED profiles, OPERCMDS profiles, or ADCD system control datasets were modified.

## Security idea

Protection without audit is incomplete. However, an audit attribute on a RACF profile is not the same as complete forensic evidence. System logging, SMF availability, and usable test identities also matter.

## What was demonstrated

1. Creation of a benign audit test dataset.
2. Creation of a closed RACF dataset profile with `UACC(NONE)`.
3. Configuration of `AUDIT(FAILURES(READ))`.
4. Temporary comparison with `AUDIT(ALL(READ))`.
5. Return to the less noisy final audit posture: `AUDIT(FAILURES(READ))`.
6. Generic DATASET refresh.
7. SMF status check showing SYS1.MAN datasets at 100% and `DUMP REQUIRED`.
8. Final sandbox comparison across PUBLIC, PRIVATE, GRANTED, WARNING and AUDIT profiles.

## Final state

```text
IBMUSER.SECLAB.AUDIT.*
  UACC(NONE)
  WARNING(NO)
  AUDIT(FAILURES(READ))
```

## Professional conclusion

This lab prepares RACF audit controls safely, but it also exposes an operational limitation: the captured SMF status shows the MAN datasets full and requiring dump. Before relying on audit trails for forensic evidence, the system logging path must be operationally healthy.
