# Evidence Walkthrough

## 1. H7USER baseline

The first screenshot confirms that `H7USER` exists as the test identity and does not show high-level administrative attributes such as `SPECIAL`, `OPERATIONS` or `AUDITOR`.

## 2. PRIVATE profile

The `IBMUSER.SECLAB.PRIVATE.*` profile shows `UACC(NONE)` and no explicit access for `H7USER`. This is the expected denial target.

## 3. AUDIT profile

The `IBMUSER.SECLAB.AUDIT.*` profile shows `UACC(NONE)` with audit configuration for read failures. This target is used to connect denial testing with audit-readiness.

## 4. SYSLOG evidence

SDSF SYSLOG was searched for `H7USER` and `ICH408I`. The violation evidence shows insufficient access authority for a protected dataset.

## 5. GRANTED counterexample

`IBMUSER.SECLAB.GRANTED.*` includes `H7USER READ`, and `H7USER` successfully browsed `IBMUSER.SECLAB.GRANTED.DATA`.

## 6. SMF status

The `/D SMF` evidence shows MAN datasets and their status. This is relevant because complete forensic workflows require RACF evidence and healthy SMF recording/processing.
