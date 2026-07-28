# Source Basis

## User evidence

- `H2.docx` contains the captured evidence for this lab.
- The document shows RACF `LISTDSD` output, benign dataset content, `LISTUSER USER2`, and the failed `USER2` TSO logon attempt.

## Conceptual basis

This lab is based on RACF dataset profile behavior:

- dataset profiles protect datasets
- `UACC` defines universal access for users not explicitly listed
- `UACC(READ)` is open-by-default read exposure
- `UACC(NONE)` is closed-by-default protection
- access lists provide explicit authorization

## Lab boundary

No DB2 commands were used.
No real subsystem started task was modified.
No production datasets were modified.
