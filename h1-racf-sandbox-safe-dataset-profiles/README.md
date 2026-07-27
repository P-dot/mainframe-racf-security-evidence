# H1 — RACF Sandbox: Safe Dataset Profiles Lab

This lab creates a safe RACF dataset-protection sandbox under `IBMUSER.SECLAB.*`.

The objective is not to audit the whole system again. The objective is to learn how RACF dataset protection can be misconfigured, how that misconfiguration can be safely demonstrated, and how to correct it using closed-by-default profiles and explicit permits.

## Scenario

A security engineer needs to understand the difference between:

- a dataset profile that is open by default: `UACC(READ)`
- a dataset profile that is closed by default: `UACC(NONE)`
- a dataset profile that is closed by default but permits one selected ID: `UACC(NONE)` + `PERMIT ... ACCESS(READ)`
- audit settings on protected resources: `AUDIT(FAILURES(READ))`

All objects are lab-owned objects under `IBMUSER.SECLAB.*`. No system libraries, APF libraries, started tasks, or production-like resources were changed.

## Lab Outcome

The lab produced three controlled RACF dataset profiles:

| Profile | Universal Access | Access List | Meaning |
|---|---:|---|---|
| `IBMUSER.SECLAB.PUBLIC.*` | `READ` | none | intentionally open profile for risk demonstration |
| `IBMUSER.SECLAB.PRIVATE.*` | `NONE` | none | closed-by-default defensive profile |
| `IBMUSER.SECLAB.GRANTED.*` | `NONE` | `USER2 READ` | least-privilege profile with explicit access |

The lab also added auditing to the private profile:

```text
ALTDSD 'IBMUSER.SECLAB.PRIVATE.*' GENERIC AUDIT(FAILURES(READ))
```

## Key Learning

`UACC` is dangerous because it applies to users who are not explicitly listed in the access list. A profile with `UACC(READ)` is useful as a controlled training example, but sensitive resources should normally be protected with `UACC(NONE)` and explicit access lists.

## Evidence

The evidence is centered on screenshots from ISPF option 6 and RACF command output. See:

```text
evidence/screenshots/
```

The original Word evidence document is preserved in:

```text
evidence/source-documents/LAB01.docx
```

## Safety Boundary

This lab did not modify:

- `SYS1.*`
- `ADCD.Z111S.*`
- APF libraries
- LINKLIST libraries
- STARTED class profiles
- OPERCMDS profiles
- OMVS root or zFS system filesystems

The sandbox is intentionally left in place for the next labs.
