# Findings

## Finding 1 — H7USER is suitable for group-based tests

`H7USER` is connected to `H7GRP`, which allows controlled testing of inherited access through RACF group membership.

## Finding 2 — GROUP profile was closed by default

`IBMUSER.SECLAB.GROUP.*` was defined with:

```text
UACC(NONE)
AUDIT(FAILURES(READ))
```

This is the correct closed-by-default baseline.

## Finding 3 — Access was granted to the group

The lab used:

```text
PERMIT 'IBMUSER.SECLAB.GROUP.*' CLASS(DATASET) ID(H7GRP) ACCESS(READ)
```

This is better than direct user-specific access for normal role-based access administration.

## Finding 4 — Group access was removed cleanly

After testing, the group permission was deleted and the profile returned to a closed state with no standard access list entries.

## Evidence note

The profile lifecycle is complete. The actual H7USER browse success/denial for `GROUP.DATA` is not as cleanly captured as the RACF profile evidence, so the lab states this limitation explicitly.

