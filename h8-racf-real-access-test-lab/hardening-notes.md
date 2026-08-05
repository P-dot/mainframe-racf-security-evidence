# Hardening Notes

Recommended defensive baseline for sensitive datasets:

```text
UACC(NONE)
WARNING(NO)
AUDIT(FAILURES(READ))
explicit access list only for required users or groups
```

Avoid:

```text
UACC(READ) for sensitive data
broad ALTER permissions
group-wide permits without justification
testing only with privileged IDs
leaving WARNING enabled permanently
```

For production, the next steps would require change control, rollback planning, SMF validation, and application-owner approval.
