# Hardening Notes

Professional RACF cleanup should follow this pattern:

```text
review -> justify -> remove -> refresh -> verify -> document
```

Do not delete profiles as a substitute for access-list cleanup.

Do not open `UACC` to solve access problems.

Do not remove working access without validating a replacement path.

Recommended review questions:

- Is this ID still active?
- Is this ID still used for this application?
- Is this a person, batch ID, service ID, or group?
- Is the access level justified?
- Can this be replaced by group-based access?
- Is the access temporary or permanent?
- Is there a ticket/change record?
