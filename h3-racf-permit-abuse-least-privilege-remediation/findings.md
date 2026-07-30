# Findings

## Finding 1 — `UACC(NONE)` is necessary but not sufficient

The `IBMUSER.SECLAB.GRANTED.*` profile is closed by default with `UACC(NONE)`. This is a good baseline, but the access list still determines who receives explicit access.

## Finding 2 — Excessive explicit access is a separate risk

The lab temporarily issued a `PERMIT` with `ACCESS(ALTER)` to show how a closed profile can still contain over-privileged entries.

## Finding 3 — Final state returns to minimum privilege

The profile was remediated back to:

```text
USER2 READ
```

This is the correct final state for a user who should only read the dataset.

## Finding 4 — Documentation lesson

The exact `USER2 ALTER` access-list line should be captured before remediation in future labs. The command history proves the action was executed, but the strongest evidence is always the post-change `LISTDSD` access-list view.
