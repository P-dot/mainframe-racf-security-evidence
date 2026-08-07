# Commands — H10

## Baseline

```text
LISTDSD DA('IBMUSER.SECLAB.PRIVATE.*') ALL
```

Expected baseline:

```text
UNIVERSAL ACCESS: NONE
No H7USER entry in standard access list
```

## Temporary access grant

```text
PERMIT 'IBMUSER.SECLAB.PRIVATE.*' CLASS(DATASET) ID(H7USER) ACCESS(READ)
SETROPTS GENERIC(DATASET) REFRESH
LISTDSD DA('IBMUSER.SECLAB.PRIVATE.*') ALL
```

Expected result:

```text
H7USER READ
```

## Access verification as H7USER

```text
ISPF 3.4
IBMUSER.SECLAB.PRIVATE.DATA
B
```

Expected result after the grant:

```text
Browse succeeds
```

## Temporary access removal

```text
PERMIT 'IBMUSER.SECLAB.PRIVATE.*' CLASS(DATASET) ID(H7USER) DELETE
SETROPTS GENERIC(DATASET) REFRESH
LISTDSD DA('IBMUSER.SECLAB.PRIVATE.*') ALL
```

Expected result:

```text
H7USER no longer appears in the access list
```

## Final denied-state verification

```text
ISPF 3.4
IBMUSER.SECLAB.PRIVATE.DATA
B
```

Expected result after removal:

```text
Authorization failed / ICH408I insufficient access authority
```
