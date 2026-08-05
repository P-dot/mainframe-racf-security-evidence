# Attack Scenario

## Controlled adversary model

`H7USER` represents a low-privileged test identity.

The question is:

```text
What can a non-privileged user read when different RACF dataset profiles are applied?
```

## Tested cases

### Open profile

```text
IBMUSER.SECLAB.PUBLIC.* -> UACC(READ)
```

A user not listed in the access list may still read through universal access.

### Closed profile

```text
IBMUSER.SECLAB.PRIVATE.* -> UACC(NONE)
```

A user without explicit access should be denied.

### Explicitly granted profile

```text
IBMUSER.SECLAB.GRANTED.* -> UACC(NONE) + H7USER READ
```

The resource is closed by default but permits a specific user.

### Audit-ready restricted profile

```text
IBMUSER.SECLAB.AUDIT.* -> UACC(NONE) + AUDIT(FAILURES(READ))
```

The profile is closed and ready to audit failed read attempts.
