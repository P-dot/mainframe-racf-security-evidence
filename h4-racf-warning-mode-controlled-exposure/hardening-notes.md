# Hardening Notes

## Do

Use `WARNING` only as a temporary implementation phase.

Recommended flow:

```text
ADDSD/RDEFINE profile
ALTDSD/RALTER WARNING
Observe attempted access
Create justified access list
ALTDSD/RALTER NOWARNING
SETROPTS refresh if required
Validate final state
```

## Do not

Do not leave production-sensitive profiles in warning mode indefinitely.

Do not treat:

```text
UACC(NONE) + WARNING(YES)
```

as equivalent to final enforcement.

## Review checklist

For every sensitive dataset profile, review:

```text
UACC
WARNING
AUDIT
access list
conditional access list
owner
last change date
```
