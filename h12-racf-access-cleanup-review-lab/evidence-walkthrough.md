# Evidence Walkthrough

## 1. Sandbox inventory

The lab starts with an attempted inventory using:

```text
SEARCH CLASS(DATASET) MASK(IBMUSER.SECLAB*)
```

The captured result shows no entries from that search expression. The lab therefore relies on direct `LISTDSD` commands against each known sandbox profile.

## 2. Profile review

The evidence reviews:

- `IBMUSER.SECLAB.PUBLIC.*`
- `IBMUSER.SECLAB.PRIVATE.*`
- `IBMUSER.SECLAB.GRANTED.*`
- `IBMUSER.SECLAB.AUDIT.*`
- `IBMUSER.SECLAB.WARNING.*`
- `IBMUSER.SECLAB.GROUP.*`

## 3. Cleanup target

`IBMUSER.SECLAB.GRANTED.*` initially shows:

```text
USER2   READ
H7USER  READ
```

`USER2` is obsolete for the current interactive testing path. `H7USER` is the real test identity.

## 4. Cleanup action

The cleanup command removes `USER2`:

```text
PERMIT 'IBMUSER.SECLAB.GRANTED.*' CLASS(DATASET) ID(USER2) DELETE
```

The generic DATASET profile cache is then refreshed:

```text
SETROPTS GENERIC(DATASET) REFRESH
```

## 5. Post-cleanup evidence

The final access list for `IBMUSER.SECLAB.GRANTED.*` shows only:

```text
H7USER  READ
```

## 6. Functional validation

`H7USER` can still browse:

```text
IBMUSER.SECLAB.GRANTED.DATA
```

and is denied on:

```text
IBMUSER.SECLAB.PRIVATE.DATA
```

The denial produces `ICH408I`, confirming that cleanup did not open the private profile.
