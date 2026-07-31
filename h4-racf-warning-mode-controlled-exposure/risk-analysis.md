# Risk Analysis

## Risk

A RACF profile left in `WARNING` mode can create a false sense of security.

The profile may appear restrictive because it has:

```text
UACC(NONE)
```

but warning mode can still allow access that would otherwise fail.

## Impact

Potential impact in a real environment:

- unauthorized access may continue while teams believe a control is enforced
- sensitive datasets may remain exposed during migration
- audit-only controls may be mistaken for preventive controls
- compliance evidence may be misleading if warning mode is not clearly documented

## Severity in this lab

```text
Low
```

because the test was limited to `IBMUSER.SECLAB.WARNING.*`.

## Severity in production

```text
Medium to High
```

depending on the data protected by the profile and the length of time `WARNING` remains active.
