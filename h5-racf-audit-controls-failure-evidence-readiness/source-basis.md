# Source Basis

This lab is based on the user's `H5.docx` evidence package, which contains screenshots of the RACF commands and the SMF status display.

Key source evidence:

- `ALLOC DATASET('IBMUSER.SECLAB.AUDIT.DATA')`
- `LISTCAT ENT('IBMUSER.SECLAB.AUDIT.DATA') ALL`
- `ADDSD 'IBMUSER.SECLAB.AUDIT.*' UACC(NONE)`
- `ALTDSD 'IBMUSER.SECLAB.AUDIT.*' AUDIT(FAILURES(READ))`
- `ALTDSD 'IBMUSER.SECLAB.AUDIT.*' AUDIT(ALL(READ))`
- Return to `AUDIT(FAILURES(READ))`
- `SETROPTS GENERIC(DATASET) REFRESH`
- `/D SMF`, showing MAN datasets full and `DUMP REQUIRED`

The lab also follows the established project rule: no destructive changes, no production datasets, and screenshots as the central evidence.
