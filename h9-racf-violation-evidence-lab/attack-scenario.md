# Attack Scenario — Controlled RACF Violation

## Scenario

A non-privileged test user, `H7USER`, attempts to read protected sandbox datasets owned by `IBMUSER`.

This simulates a low-privilege user attempting to access data outside their authorized scope.

## Controlled target

```text
IBMUSER.SECLAB.PRIVATE.DATA
IBMUSER.SECLAB.AUDIT.DATA
```

## Expected outcome

The access attempt should fail because:

```text
UACC(NONE)
no explicit H7USER access list entry
```

## Evidence objective

The defensive evidence is the RACF violation message:

```text
ICH408I
```

The message identifies the user, group, resource class, dataset name, requested access and allowed access.
