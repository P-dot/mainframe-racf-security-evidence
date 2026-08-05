# Rollback Notes

Use rollback only if the test identity must be removed.

## Revoke only

```text
ALTUSER H7USER REVOKE
```

## Remove sandbox permission

```text
PERMIT 'IBMUSER.SECLAB.GRANTED.*' CLASS(DATASET) ID(H7USER) DELETE
SETROPTS GENERIC(DATASET) REFRESH
```

## Remove ACCTNUM authorization

```text
PERMIT ACCT# CLASS(ACCTNUM) ID(H7USER) DELETE
SETROPTS RACLIST(ACCTNUM) REFRESH
```

## Full cleanup

Only after confirming no later labs depend on the user:

```text
DELUSER H7USER
DELGROUP H7GRP
```

Do not delete `H7USER` if the next labs will use it.
