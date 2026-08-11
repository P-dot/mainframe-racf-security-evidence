# Findings

## Finding 1 — USER2 was obsolete in the sandbox access list

`IBMUSER.SECLAB.GRANTED.*` showed both `USER2 READ` and `H7USER READ`.

Since `USER2` was not the working interactive test identity, it was a cleanup candidate.

## Finding 2 — Cleanup preserved legitimate access

After removing `USER2`, `H7USER READ` remained present.

`H7USER` was still able to browse `IBMUSER.SECLAB.GRANTED.DATA`.

## Finding 3 — Cleanup did not weaken PRIVATE protection

`IBMUSER.SECLAB.PRIVATE.*` remained protected by `UACC(NONE)`.

`H7USER` continued to receive `ICH408I` when attempting to access `IBMUSER.SECLAB.PRIVATE.DATA`.

## Finding 4 — GROUP profile was reviewed

`IBMUSER.SECLAB.GROUP.*` was reviewed to ensure no residual group access remained from the prior group-based access lab.

## Finding 5 — SEARCH syntax did not return the inventory

The captured `SEARCH CLASS(DATASET) MASK(IBMUSER.SECLAB*)` returned no entries. This does not invalidate the lab because the direct `LISTDSD` commands supplied the required profile evidence.

Operational lesson: when `SEARCH` does not return expected generic DATASET profiles, use explicit `LISTDSD DA('profile') ALL` commands for controlled verification.
