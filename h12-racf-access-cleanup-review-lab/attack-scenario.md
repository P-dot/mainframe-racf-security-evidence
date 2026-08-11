# Attack Scenario

The controlled security scenario is not a destructive attack. It is an access-list hygiene problem.

A profile can look secure because it has:

```text
UACC(NONE)
```

but still contain unnecessary entries in its access list.

In this lab, `USER2` was still present on `IBMUSER.SECLAB.GRANTED.*` even though `H7USER` had become the real test identity for interactive RACF access testing.

Security issue:

```text
obsolete explicit access remains after the identity is no longer used
```

Potential real-world impact:

- orphaned test IDs retain access,
- former project IDs retain data access,
- access reviews become noisy,
- least privilege drifts over time,
- manual exceptions accumulate.
