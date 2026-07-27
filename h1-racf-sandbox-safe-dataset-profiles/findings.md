# Findings — H1 RACF Sandbox Dataset Profiles

## H1-F01 — Safe RACF Sandbox Created

Three controlled datasets were created under:

```text
IBMUSER.SECLAB.*
```

Evidence:

- `06_public_dataset_cataloged.png`
- `07_private_dataset_cataloged.png`
- `08_granted_dataset_cataloged.png`

Security meaning: the lab uses safe test objects and does not touch system resources.

## H1-F02 — Open Universal Access Demonstrated

Profile:

```text
IBMUSER.SECLAB.PUBLIC.*
```

Result:

```text
UACC(READ)
```

Evidence:

- `11_public_profile_uacc_read_top.png`
- `19_final_public_uacc_read.png`

Security meaning: this simulates a risky configuration where users not listed in the access list may still read the protected resource.

## H1-F03 — Closed-by-Default Protection Demonstrated

Profile:

```text
IBMUSER.SECLAB.PRIVATE.*
```

Result:

```text
UACC(NONE)
```

Evidence:

- `13_private_profile_uacc_none_top.png`
- `21_final_private_uacc_none_audit.png`

Security meaning: this is the defensive pattern. Access is denied by default unless explicitly permitted.

## H1-F04 — Explicit Least-Privilege Access Demonstrated

Profile:

```text
IBMUSER.SECLAB.GRANTED.*
```

Result:

```text
UACC(NONE)
USER2 READ
```

Evidence:

- `15_granted_profile_uacc_none_top.png`
- `16_granted_user2_read_access_list.png`
- `24_final_granted_user2_read.png`

Security meaning: this is the correct least-privilege pattern for granting limited read access.

## H1-F05 — Audit Setting Added to Private Profile

Command:

```text
ALTDSD 'IBMUSER.SECLAB.PRIVATE.*' GENERIC AUDIT(FAILURES(READ))
```

Result:

```text
AUDITING
FAILURES(READ)
```

Evidence:

- `17_private_profile_audit_failures_read_top.png`
- `21_final_private_uacc_none_audit.png`

Security meaning: deny-by-default should be paired with visibility into failed access attempts.

## H1-F06 — Syntax Correction Captured

Initial command:

```text
ADDSD 'IBMUSER.SECLAB.PUBLIC.**' GENERIC UACC(READ)
```

Result:

```text
IKJ56702I INVALID DATASET NAME
```

Corrected command:

```text
ADDSD 'IBMUSER.SECLAB.PUBLIC.*' UACC(READ)
```

Evidence:

- `09_failed_addsd_double_star_generic.png`
- `11_public_profile_uacc_read_top.png`

Security meaning: the lab documents not only the final state but also the real diagnostic path.
