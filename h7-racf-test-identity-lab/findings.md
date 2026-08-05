# Findings

## Finding 1 — IBMUSER is unsuitable as a normal test actor

`IBMUSER` has elevated authority and can mask normal RACF denials.

## Finding 2 — USER2 was not enough for realistic testing

Earlier evidence showed `USER2` existed but could not use TSO. A new controlled identity was needed.

## Finding 3 — H7USER is a controlled non-privileged identity

`H7USER` was created with `H7GRP` as default group and no visible global privileged attributes in the captured evidence.

## Finding 4 — TSO logon requires more than ADDUSER

A usable TSO identity needs a valid TSO segment and account-number authorization.

## Finding 5 — ACCTNUM protection affected logon

TSO logon failed until `H7USER` was permitted to use `ACCT#` in class `ACCTNUM`.

## Finding 6 — H7USER can now be used for later denial and audit labs

The ISPF Primary Option Menu confirms that `H7USER` reached an interactive session.
