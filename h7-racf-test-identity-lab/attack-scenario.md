# Attack Scenario

`IBMUSER` is too powerful to represent a normal attacker or normal application user. If all tests are executed with `IBMUSER`, denied access paths may not be visible because the account has elevated authority.

This lab creates a weak, controlled test identity. That identity becomes the controlled actor for later labs.

The security question becomes:

```text
What can a normal RACF identity actually read, update, or fail to access?
```

This makes later tests more realistic and prevents the lab from confusing administrative authority with normal user access.
