# Commands — H2 UACC Abuse Lab

Run location:

```text
ISPF option 6
```

Dataset navigation/content editing:

```text
ISPF option 3.4
```

## Profile review

```text
SEARCH CLASS(DATASET) MASK(IBMUSER.SECLAB*)
```

```text
LISTDSD DA('IBMUSER.SECLAB.PUBLIC.*') ALL
```

```text
LISTDSD DA('IBMUSER.SECLAB.PRIVATE.*') ALL
```

```text
LISTDSD DA('IBMUSER.SECLAB.GRANTED.*') ALL
```

## Test dataset content

In `IBMUSER.SECLAB.PUBLIC.DATA`:

```text
PUBLIC TEST DATASET
THIS FILE SIMULATES A DATASET THAT SHOULD NOT BE OPEN TO EVERYONE.
NO REAL SENSITIVE INFORMATION IS STORED HERE.
```

In `IBMUSER.SECLAB.PRIVATE.DATA`:

```text
PRIVATE TEST DATASET
THIS FILE SIMULATES A RESTRICTED DATASET.
ACCESS SHOULD BE DENIED UNLESS EXPLICITLY GRANTED.
```

In `IBMUSER.SECLAB.GRANTED.DATA`:

```text
GRANTED TEST DATASET
THIS FILE SIMULATES LEAST PRIVILEGE ACCESS.
ONLY EXPLICITLY PERMITTED USERS SHOULD READ IT.
```

## USER2 validation

```text
LISTUSER USER2
```

Attempted TSO logon with `USER2` resulted in:

```text
IKJ56420I Userid USER2 not authorized to use TSO
```

## Notes

No destructive changes were performed in this lab.

The vulnerable `PUBLIC` profile was intentionally left as-is for the next remediation lab.
