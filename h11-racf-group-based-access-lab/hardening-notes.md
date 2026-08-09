# Hardening Notes

Recommended production pattern:

```text
ADDSD 'APP.DATA.*' UACC(NONE)
PERMIT 'APP.DATA.*' CLASS(DATASET) ID(APPREAD) ACCESS(READ)
PERMIT 'APP.DATA.*' CLASS(DATASET) ID(APPUPDT) ACCESS(UPDATE)
```

Then administer users through group membership rather than direct permits.

Do not use `UACC(READ)` as a shortcut for business access.

