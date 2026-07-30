# Risk Analysis

## Risk statement

A RACF DATASET profile can be closed universally but still expose risk through excessive explicit permissions.

## Risk path

```text
UACC(NONE)
+ explicit access list entry
+ unnecessary ALTER authority
= least privilege failure
```

## Impact in a real environment

If excessive `ALTER` access were granted to a sensitive dataset profile, a user could potentially perform actions beyond their operational need. This increases the impact of compromised credentials, human error, or privilege misuse.

## Lab impact

No production impact. The test was limited to:

```text
IBMUSER.SECLAB.GRANTED.*
```

## Severity

Medium in the sandbox. High if the same pattern exists on system, application, APF, JES, security database, or operational datasets.
