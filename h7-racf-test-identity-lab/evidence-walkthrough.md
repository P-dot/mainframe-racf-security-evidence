# Evidence Walkthrough

## 1. Clean baseline

The evidence shows that `H7GRP` and `H7USER` did not exist at the start of the lab.

## 2. TSO reference

`LISTUSER IBMUSER TSO` was used to identify a working TSO pattern in this ADCD environment:

```text
PROC=DBSPROC9
ACCTNUM=ACCT#
UNIT=3390
COMMAND=ISPF
```

## 3. Group and user creation

`H7GRP` was created under `SYS1`, and `H7USER` was created with `H7GRP` as default group.

## 4. TSO segment troubleshooting

The initial `ALTUSER` command required adjustment because `SIZE(02048000)` was rejected as invalid. The command was corrected using `SIZE(2048000)` and `MAXSIZE(2096128)`.

## 5. ACCTNUM troubleshooting

The first TSO logon reached the TSO/E logon panel but failed because the account number was not authorized for the user. The remediation was:

```text
PERMIT ACCT# CLASS(ACCTNUM) ID(H7USER) ACCESS(READ)
```

## 6. Successful logon

The evidence shows `H7USER` reaching the ISPF Primary Option Menu. This confirms the user can now act as a real non-privileged test identity.

## 7. Sandbox permission

The `IBMUSER.SECLAB.GRANTED.*` profile shows `H7USER READ`, preparing the next lab for positive and negative access testing.
