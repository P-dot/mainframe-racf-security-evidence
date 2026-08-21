# Evidence Index

The screenshots in this directory originate from the Lab 26 evidence document and the final standalone SDSF captures.

Key evidence to retain:

- `SETROPTS LIST` — class configuration baseline.
- `SEARCH CLASS(OPERCMDS)` — profile inventory.
- `RLIST OPERCMDS ...` — UACC, access, warning and audit attributes.
- `LU IBMUSER` — RACF identity context.
- `D A,L` — successful read-only system display.
- `D IPLINFO` — successful read-only IPL display.
- `D OPDATA` — successful read-only operator data display.
- `lab26-setprog-security-rejection.png` — `IEE345I ... FAILED BY SECURITY`.
- `lab26-sdsf-who-context.png` — SDSF `WHO` execution context.

No network-address, MAC-address or host-adapter evidence is intentionally documented by this lab.
