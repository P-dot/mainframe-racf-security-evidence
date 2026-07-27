# Risk Analysis — H1 RACF Sandbox Dataset Profiles

## Risk 1 — Open UACC on Sensitive Data

A dataset profile with:

```text
UACC(READ)
```

can expose data to users who were never explicitly placed in the access list.

In this lab, that risk is intentionally emulated with:

```text
IBMUSER.SECLAB.PUBLIC.*
```

Production impact if this pattern existed on sensitive resources:

- unauthorized data disclosure
- lateral movement through readable configuration/data files
- weak separation of duties
- possible leakage through batch SYSIN/SYSOUT or application datasets

## Risk 2 — Missing Audit on Denied Access

A protected resource with no audit settings might block access but still provide weak detection.

The lab improves this by adding:

```text
AUDIT(FAILURES(READ))
```

on the private profile.

Production impact if missing:

- failed reconnaissance attempts might not be visible
- investigation evidence might be incomplete
- security monitoring might miss access probing

## Risk 3 — Syntax and Generic Naming Assumptions

The failed `ADDSD` command shows why RACF syntax must match the environment.

The failed pattern used:

```text
IBMUSER.SECLAB.PUBLIC.**
```

The corrected pattern used:

```text
IBMUSER.SECLAB.PUBLIC.*
```

Production impact:

- incorrect generic profile definitions can leave resources unprotected
- an administrator might believe a control exists when the command actually failed
- screenshots and verification are essential

## Risk 4 — Verification Gaps

The final `SEARCH CLASS(DATASET) MASK(IBMUSER.SECLAB*)` did not list the created profiles in the captured evidence, while profile-specific `LISTDSD` clearly showed the profiles.

Production impact:

- different RACF commands answer different questions
- one verification command is not enough
- use direct `LISTDSD` verification for specific profiles
