# Risk Analysis

## Risk demonstrated

Access-list drift.

Over time, RACF profiles can retain individual users or groups that no longer need access. This creates unnecessary exposure even when the profile uses `UACC(NONE)`.

## Why it matters

A security review that only checks `UACC(NONE)` is incomplete. The access list must also be reviewed.

A profile with closed universal access can still be risky if stale entries remain.

## Risk level in this lab

Low, because this is a sandbox under:

```text
IBMUSER.SECLAB.*
```

## Risk in production

Medium to high, depending on the sensitivity of the protected datasets and the privileges of the stale identities.

Examples:

- old support IDs with `UPDATE`,
- batch IDs with access to production data,
- groups left behind after migrations,
- test users with access to application files,
- former project users retained in access lists.
