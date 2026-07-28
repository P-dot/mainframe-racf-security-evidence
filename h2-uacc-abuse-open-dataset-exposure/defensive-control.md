# Defensive Control — Closed by Default

## Weak control

```text
UACC(READ)
```

This is open-by-default read access.

## Better control

```text
UACC(NONE)
```

This denies universal access.

## Professional control

```text
UACC(NONE)
PERMIT ... ID(user-or-group) ACCESS(READ|UPDATE|CONTROL|ALTER)
```

This gives access only to named users or groups.

## Evidence from H2

The lab compares three profiles:

```text
PUBLIC   -> UACC(READ)
PRIVATE  -> UACC(NONE)
GRANTED  -> UACC(NONE) + USER2 READ
```

This is the foundation for least privilege in RACF dataset protection.
