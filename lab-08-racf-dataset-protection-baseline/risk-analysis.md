# Risk Analysis

## Why these datasets matter

| Dataset type | Security relevance |
|---|---|
| `PARMLIB` | Controls system parameters and startup configuration. |
| `PROCLIB` | Contains JCL procedures, including started task procedures. |
| `LINKLIB` | Contains executable system modules. |
| `LPALIB` | Contains modules loaded into link pack area. |
| `NUCLEUS` | Core system dataset. |
| `VTAMLIB` | Network/VTAM-related runtime libraries. |
| zFS datasets | Back z/OS UNIX directories and product filesystems. |

## Risk level in this lab

This is a lab ADCD system, so the lack of visible profiles is not unexpected.

For production, the finding would be serious because system datasets should not depend only on operating conventions or physical access. They should have documented RACF protection, controlled access lists, and audit settings.

## Main security concerns

1. System libraries without RACF profiles can weaken accountability.
2. Executable libraries such as `LINKLIB` and `LPALIB` are especially sensitive.
3. `PROCLIB` controls started task definitions; weak protection can affect service identity and startup behavior.
4. zFS datasets should be reviewed because z/OS UNIX authority and MVS dataset authority meet at this layer.
5. This lab connects directly with the UID(0) exposure found in Lab 07.

## Production hardening direction

This lab does not apply hardening. It identifies the baseline.

Potential future actions, only after backup and change-control planning, include:

- Define dataset profiles for system libraries.
- Use UACC(NONE) or tightly controlled access where appropriate.
- Avoid broad `ID(*)` permissions for sensitive datasets.
- Add auditing for sensitive changes/access attempts.
- Review zFS backing datasets individually.
- Validate with RACF Health Checker and, if available, DSMON or zSecure.
