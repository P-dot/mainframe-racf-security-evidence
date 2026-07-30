# Defensive Control — Least Privilege in RACF PERMIT

## Control principle

Do not rely only on `UACC(NONE)`. Review and justify every entry in the access list.

## Defensive state

```text
IBMUSER.SECLAB.GRANTED.*
UACC(NONE)
USER2 READ
```

This means:

- the profile is closed by default;
- `USER2` has only the access required for the scenario;
- `ALTER` is removed because it is not justified.

## Remediation command

```text
PERMIT 'IBMUSER.SECLAB.GRANTED.*' CLASS(DATASET) ID(USER2) ACCESS(READ)
SETROPTS GENERIC(DATASET) REFRESH
```

## Professional review checklist

For every sensitive profile, review:

```text
UACC
WARNING
AUDIT
standard access list
conditional access list
group-based access
users with ALTER or CONTROL
whether each permission has a business justification
```
