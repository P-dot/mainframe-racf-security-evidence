# H2 — UACC Abuse Lab: Open Dataset Exposure

This lab demonstrates why `UACC(READ)` is a dangerous default for RACF dataset profiles.

The work uses the safe sandbox created in H1 under `IBMUSER.SECLAB.*`. No production datasets, system libraries, DB2 objects, started tasks, APF libraries, or `SYS1.*` / `ADCD.Z111S.*` resources were modified.

## Learning objective

Understand the difference between:

| Profile | Security pattern | Meaning |
|---|---|---|
| `IBMUSER.SECLAB.PUBLIC.*` | `UACC(READ)` | Open-by-default read exposure |
| `IBMUSER.SECLAB.PRIVATE.*` | `UACC(NONE)` | Closed-by-default dataset protection |
| `IBMUSER.SECLAB.GRANTED.*` | `UACC(NONE)` + `USER2 READ` | Least privilege with explicit access |

## Scenario

A RACF dataset profile with `UACC(READ)` allows universal read access to users not otherwise restricted by stronger controls. This may expose JCL, parameters, application data, technical names, paths, SYSOUT-like content, or operational clues.

In this lab, the exposure is demonstrated with harmless test datasets only.

## Key result

The evidence shows:

- `PUBLIC` profile is readable by universal access: `UACC(READ)`.
- `PRIVATE` profile is closed by default: `UACC(NONE)`.
- `GRANTED` profile is closed by default and has an explicit `USER2 READ` access-list entry.
- `USER2` exists, but the captured environment does not allow `USER2` to use TSO, so the lab remains a profile-analysis lab rather than a live cross-user browse test.

## Why this matters

The defensive pattern is not “make the dataset accessible and hope users behave”.

The defensive pattern is:

```text
UACC(NONE)
+
explicit PERMIT entries
+
audit of failures where appropriate
```

This lab is the controlled offensive-defensive explanation of why open universal read access is a real weakness.
