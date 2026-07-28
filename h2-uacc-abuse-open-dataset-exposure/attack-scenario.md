# Attack Scenario — Open Universal Read Access

## Conceptual abuse

A profile like this is weak:

```text
IBMUSER.SECLAB.PUBLIC.*  UACC(READ)
```

The weakness is not code execution. The weakness is information exposure.

A user who is not explicitly listed in the access list may still read datasets covered by the profile because the universal access level already grants read authority.

## What an attacker gains

In a real environment, a readable dataset can reveal:

- JCL structure
- program names
- DD names
- dataset naming standards
- operational paths
- job parameters
- credentials or tokens if poor practices exist
- subsystem names
- application architecture clues

## Safe lab boundary

This lab does not read sensitive data. It only places benign demonstration text in sandbox datasets under `IBMUSER.SECLAB.*`.
