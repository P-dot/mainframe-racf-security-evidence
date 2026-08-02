# Evidence Walkthrough

## Pages 1-2 — Dataset and profile baseline

The evidence shows creation of `IBMUSER.SECLAB.AUDIT.DATA`, `LISTCAT` confirmation, `ADDSD 'IBMUSER.SECLAB.AUDIT.*' UACC(NONE)`, and a `LISTDSD` result showing `UNIVERSAL ACCESS NONE`, `WARNING NO`, and no standard access list.

## Pages 3-4 — Audit configuration comparison

The evidence shows `ALTDSD 'IBMUSER.SECLAB.AUDIT.*' AUDIT(FAILURES(READ))`, followed by `LISTDSD` showing `AUDITING FAILURES(READ)`. Then the profile is changed to `AUDIT(ALL(READ))`, and `LISTDSD` shows `AUDITING ALL(READ)`.

## Pages 5-6 — Return to focused failure auditing

The command `ALTDSD 'IBMUSER.SECLAB.AUDIT.*' AUDIT(FAILURES(READ))` is entered again. The later `LISTDSD` output confirms the final intended audit setting.

## Page 7 — SMF operational context

The SMF display shows:

```text
SMF DATA SETS 3
SYS1.MAN1 100 DUMP REQUIRED
SYS1.MAN2 100 DUMP REQUIRED
SYS1.MAN3 100 DUMP REQUIRED
```

This is a key finding: RACF audit settings exist, but the SMF logging path needs operational attention before the system can be treated as fully forensic-ready.

## Pages 7-12 — Final sandbox comparison

The final comparison shows:

```text
PUBLIC   -> UACC(READ)
PRIVATE  -> UACC(NONE)
GRANTED  -> UACC(NONE) + USER2 READ
WARNING  -> UACC(NONE) + WARNING(NO) + AUDIT(ALL(READ)) from H4
AUDIT    -> UACC(NONE) + AUDIT(FAILURES(READ))
```

The final H5 profile is correctly left in failure-auditing mode.
