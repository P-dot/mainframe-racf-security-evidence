# Defensive Control

The defensive control is identity minimization.

`H7USER` is deliberately created without:

- `SPECIAL`
- `OPERATIONS`
- `AUDITOR`
- `UID(0)`
- broad group authority
- production dataset permissions
- APF or system command authority

The user is only given what is required for the next stage of controlled testing:

- TSO logon capability
- ISPF startup through `DBSPROC9`
- access to the TSO account number `ACCT#`
- read access to the sandbox profile `IBMUSER.SECLAB.GRANTED.*`
