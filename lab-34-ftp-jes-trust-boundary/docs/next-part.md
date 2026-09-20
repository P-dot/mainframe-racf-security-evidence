# Lab 34 — Part 2 Planned Continuation

## Starting state

Part 1 established all of the following independently:

```text
FTP authentication             observed
SITE FILE=JES                  observed
internal-reader handoff        observed
JES job identifier             observed
SDSF authorization denial      observed separately
JCL-processing failure         observed separately
```

Part 2 must not repeat basic FTP discovery unless required to reproduce the test.

## Questions to resolve

1. Which exact JCL statement generated `IEF650I UNIDENTIFIED OPERATION FIELD` in the captured run?
2. Can a freshly validated, fixed-format harmless `IEFBR14` job pass JES JCL conversion unchanged through FTP?
3. Once JCL is valid, what identity does JES associate with the submitted job?
4. Which active RACF/SAF/JES controls apply to that identity and workload in this installation?
5. Is any denial observed at the execution/resource boundary, and which exact control produces it?
6. What does the SDSF `NO GROUP ASSIGNMENT` result mean independently of job submission?
7. If a minimum controlled authorization change is justified, can it be validated and rolled back?
8. Can rollback be proven with a repeat negative test?

## Target sequence

```text
PART 1 BASELINE                         COMPLETE
FTP -> JES internal reader -> JOB id

JCL ROOT-CAUSE                         NEXT
        |
        v
VALID HARMLESS IEFBR14 JOB
        |
        v
FTP -> JES RETEST
        |
        v
CLASSIFY JES/RACF RESULT
        |
        +--> denied: identify exact control
        |
        +--> allowed: prove completion only
        |
        v
MINIMUM CHANGE ONLY IF REQUIRED
        |
        v
ROLLBACK
        |
        v
RETEST
```

## Safety constraints

- Do not grant `SPECIAL`, `OPERATIONS`, `AUDITOR`, UID(0), or equivalent broad privilege.
- Do not disable or weaken unrelated RACF classes/profiles.
- Do not replace the test identity with an administrator merely to manufacture success.
- Do not publish credentials, private network data, host adapter identifiers, or unrelated infrastructure details.
- Preserve the Part 1 evidence unchanged as the original baseline.
