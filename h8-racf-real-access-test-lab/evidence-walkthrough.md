# Evidence Walkthrough

## 01 — H7USER identity baseline

The screenshots show `H7USER` exists and is not carrying privileged attributes such as `SPECIAL`, `OPERATIONS`, or `AUDITOR`.

## 02 — PUBLIC profile

`IBMUSER.SECLAB.PUBLIC.*` shows:

```text
UNIVERSAL ACCESS: READ
```

This explains why `H7USER` can browse `PUBLIC.DATA`.

## 03 — PRIVATE profile

`IBMUSER.SECLAB.PRIVATE.*` shows:

```text
UNIVERSAL ACCESS: NONE
```

The later browse attempt produces an access denial / ICH408I-style evidence.

## 04 — GRANTED profile

`IBMUSER.SECLAB.GRANTED.*` shows:

```text
UNIVERSAL ACCESS: NONE
H7USER READ
```

This explains why `H7USER` can browse `GRANTED.DATA`.

## 05 — AUDIT profile

`IBMUSER.SECLAB.AUDIT.*` shows:

```text
UNIVERSAL ACCESS: NONE
AUDITING: FAILURES(READ)
```

This profile is closed and prepared for audit of failed read attempts.

## 06 — H7USER session proof

`PROFILE` confirms the active prefix/session context for `H7USER`.

## 07 — Real tests

The evidence shows:

```text
PUBLIC.DATA browse success
PRIVATE.DATA access denied
GRANTED.DATA browse success
```

An additional SDSF LOG search shows `ICH408I` evidence associated with access failure.
