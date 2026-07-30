# Rollback Notes

## Final lab state

The intended final state is already the remediated state:

```text
PERMIT 'IBMUSER.SECLAB.GRANTED.*' CLASS(DATASET) ID(USER2) ACCESS(READ)
SETROPTS GENERIC(DATASET) REFRESH
```

## Rollback to remove USER2 access completely

Only use this if the next lab requires a clean access list:

```text
PERMIT 'IBMUSER.SECLAB.GRANTED.*' CLASS(DATASET) ID(USER2) DELETE
SETROPTS GENERIC(DATASET) REFRESH
```

## Do not delete the sandbox yet

Keep `IBMUSER.SECLAB.*` for the next labs. It will be reused for WARNING, AUDIT, and access-failure exercises.
