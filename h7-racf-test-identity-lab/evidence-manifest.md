# Evidence Manifest

This lab uses screenshots extracted from the H7 Word evidence documents.

Key evidence observed:

| Evidence | Meaning |
|---|---|
| H7GRP not found before creation | Clean baseline before creating the lab group |
| H7USER not found before creation | Clean baseline before creating the lab user |
| IBMUSER TSO reference | Working TSO parameters were copied conceptually from IBMUSER |
| H7GRP created | RACF lab group exists under SYS1 with IBMUSER owner |
| H7USER created | RACF lab user exists with default group H7GRP and no global privileges visible |
| H7USER TSO segment | TSO segment configured with DBSPROC9, ACCT#, 3390 and ISPF |
| ACCTNUM ACCT# access | H7USER was permitted to use the account number required by TSO logon |
| ISPF Primary Option Menu as H7USER | Successful interactive logon as the test identity |
| IBMUSER.SECLAB.GRANTED.* access list | H7USER has READ access to the controlled sandbox dataset profile |

The raw Word source documents are not stored in the package to avoid publishing temporary credential setup details.
