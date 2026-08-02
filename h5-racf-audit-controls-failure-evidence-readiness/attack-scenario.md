# Attack Scenario

This lab does not perform a destructive attack. It models the defensive problem an attacker would exploit indirectly:

```text
A dataset can be protected, but if failures are not audited, attempted access may leave poor evidence.
```

The controlled scenario is:

1. A restricted dataset profile exists with `UACC(NONE)`.
2. Read attempts should be controlled.
3. RACF auditing is configured so failed read attempts are audit-relevant.
4. The system logging path is checked to determine whether the environment is ready to preserve evidence.

## Why this matters

In a real z/OS security investigation, the question is not only:

```text
Was access denied?
```

It is also:

```text
Was the attempted access recorded?
Can the security team retrieve and trust the evidence?
Is SMF healthy enough to support forensic review?
```
