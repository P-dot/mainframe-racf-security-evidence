# Rollback Notes

## Current safe final state

The lab ends with:

```text
ALTDSD 'IBMUSER.SECLAB.WARNING.*' NOWARNING
SETROPTS GENERIC(DATASET) REFRESH
```

## If WARNING is accidentally left active

Run:

```text
ALTDSD 'IBMUSER.SECLAB.WARNING.*' NOWARNING
SETROPTS GENERIC(DATASET) REFRESH
LISTDSD DA('IBMUSER.SECLAB.WARNING.*') ALL
```

Confirm:

```text
WARNING: NO
```

## If audit needs to be removed later

This lab keeps audit enabled because it is useful for learning.

A future cleanup lab can decide whether to reduce or remove audit settings.
