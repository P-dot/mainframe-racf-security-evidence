# Source basis

This lab builds on the previous sandbox sequence:

- H7 created the non-privileged test identity `H7USER`.
- H8 validated real access behavior with `H7USER`.
- H9 captured RACF violation evidence with `ICH408I`.
- H10 adds temporary access, validates it, removes it, and validates denial again.

The technical basis is RACF DATASET profile administration using `LISTDSD`, `PERMIT`, and `SETROPTS GENERIC(DATASET) REFRESH`.
