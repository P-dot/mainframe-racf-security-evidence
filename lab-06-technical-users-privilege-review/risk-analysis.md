# Risk Analysis

## Privileged attributes

`SPECIAL`, `OPERATIONS`, and `AUDITOR` are the first things to check in a RACF user profile.

- `SPECIAL` means broad RACF administrative control.
- `OPERATIONS` can allow broad operational access to protected resources.
- `AUDITOR` is sensitive because it controls or reviews auditing.

In this lab:

- `IBMUSER` has `SPECIAL OPERATIONS`.
- `START1` has `OPERATIONS` and `PROTECTED`.
- `START2` has `PROTECTED` but no global `OPERATIONS` shown.
- `TCPIP`, `FTPD`, `WEBSRV`, and `OMVSKERN` show `ATTRIBUTES=NONE`.

## Technical account protection

A service ID used by a started task should normally not be used for normal human logon. The `PROTECTED` attribute is a good indicator of this intention.

In this lab:

- `START1` and `START2` are protected.
- `TCPIP`, `FTPD`, `WEBSRV`, and `OMVSKERN` are not shown as protected in captured evidence.

## Concentration of service ownership

The previous started-task lab showed that many services run under `START2`. This creates operational simplicity but weaker accountability.

A hardened model would prefer:

- one service ID per critical subsystem or product
- minimum required permissions
- no shared generic service ID for unrelated critical services
- protected service IDs
- audited administrative changes

## Lab versus production

These results are acceptable as an ADCD educational baseline. They should not be presented as a hardened production RACF design.
