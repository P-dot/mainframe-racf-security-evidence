# RACF Notes from Lab 01

## Commands used

```text
LISTUSER IBMUSER
LISTGRP SYS1
```

## RACF terms observed

| Term | Meaning |
|---|---|
| USER | RACF user identity |
| OWNER | Owner/administrator of the RACF profile |
| DEFAULT-GROUP | Default RACF group for the user |
| SPECIAL | RACF security administration privilege |
| OPERATIONS | Broad operational access privilege |
| REVOKE DATE=NONE | Account is not revoked |
| CLASS AUTHORIZATIONS=NONE | No specific class authority shown in this field |
| ACCESS=JOIN | High authority within the RACF group |
| ACCESS=USE | Basic group usage authority |

## Main learning

A RACF review is not only about running a command. The value is in interpreting the output:

```text
Command -> Evidence -> Meaning -> Risk -> Recommendation
```
