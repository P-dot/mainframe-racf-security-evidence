# Risk analysis

## Bad remediation pattern

```text
ALTDSD 'IBMUSER.SECLAB.PRIVATE.*' UACC(READ)
```

This would open the resource broadly and would not be acceptable for a restricted dataset.

## Good remediation pattern

```text
PERMIT 'IBMUSER.SECLAB.PRIVATE.*' CLASS(DATASET) ID(H7USER) ACCESS(READ)
```

This grants only what is required, to only the user that requires it.

## Residual risk

Temporary permissions become risky when they are not removed. The important control is not only the initial grant, but also the removal and final verification.
