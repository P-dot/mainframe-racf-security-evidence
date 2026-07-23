# Risk Analysis

## Risk level in this lab

Medium to high as a security observation, but expected for an ADCD training environment.

## Why it matters

UID 0 is z/OS UNIX superuser authority. A user with UID 0 passes z/OS UNIX security checks and can perform privileged UNIX functions. When several IDs share UID 0, z/OS UNIX treats them as the same UNIX identity for authorization checking, which reduces individual accountability.

## Highest-risk IDs observed

| ID | Reason |
|---|---|
| IBMUSER | `SPECIAL OPERATIONS` plus UID 0. Lab superuser. |
| START1 | `OPERATIONS`, `PROTECTED`, UID 0. Used by started tasks. |
| OMVSKERN | UID 0 and `/bin/sh`. Kernel/OMVS-related technical ID. |
| TCPIP | UID 0 and network service role. |
| FTPD | UID 0 and FTP service role. |
| WEBSRV | UID 0, `/bin/sh`, web service role. |
| START2 | Appears in UNIXMAP U0, but direct OMVS output must be captured. |

## Production hardening considerations

In production, the expected direction would be:

1. Minimize the number of UID 0 users.
2. Avoid shared UNIX identities unless strictly required.
3. Prefer granular UNIXPRIV profiles where appropriate.
4. Review BPX.* FACILITY profiles for superuser and server/daemon authority.
5. Ensure technical started-task IDs are PROTECTED.
6. Ensure services have separate technical IDs and documented permissions.
7. Capture SMF/audit evidence for changes and privileged usage.

No hardening changes were performed in this lab.
