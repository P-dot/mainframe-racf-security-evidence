# Findings

## Finding 1 — PRIVATE profile is closed by default

`IBMUSER.SECLAB.PRIVATE.*` uses `UACC(NONE)`. This prevents universal access to the protected dataset.

## Finding 2 — H7USER is denied before explicit authorization

Before the temporary grant, `H7USER` cannot browse `IBMUSER.SECLAB.PRIVATE.DATA`. The denial is visible as an authorization failure and `ICH408I` evidence.

## Finding 3 — Specific READ access resolves the denial without opening the profile

Granting `H7USER READ` allows the user to browse the dataset while preserving `UACC(NONE)`.

## Finding 4 — Removing the access-list entry restores denial

After `PERMIT ... DELETE`, `H7USER` is again denied access. This confirms the exception lifecycle is reversible and controlled.
