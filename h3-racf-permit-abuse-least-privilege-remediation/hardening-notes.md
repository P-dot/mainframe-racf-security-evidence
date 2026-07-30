# Hardening Notes

## Recommended pattern

Use a closed-by-default model:

```text
UACC(NONE)
```

Then grant only the required access:

```text
READ    for users who only need to view
UPDATE  only when content modification is required
ALTER   only for owners, administrators, or tightly controlled automation
```

## Access review rule

Every `ALTER` entry should have a written justification.

## Production caution

Do not mass-change existing RACF permissions on production systems. Review dependencies, owner groups, batch jobs, started tasks, application IDs, and rollback procedures first.
