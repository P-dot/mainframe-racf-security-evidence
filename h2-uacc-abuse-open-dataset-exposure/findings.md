# Findings

## Finding 1 — Public dataset profile is open by default

`IBMUSER.SECLAB.PUBLIC.*` is configured with:

```text
UACC(READ)
```

This is intentionally vulnerable for the lab. It represents read exposure by default.

## Finding 2 — Private dataset profile follows closed-by-default design

`IBMUSER.SECLAB.PRIVATE.*` is configured with:

```text
UACC(NONE)
```

This is the defensive baseline.

## Finding 3 — Granted dataset profile demonstrates least privilege

`IBMUSER.SECLAB.GRANTED.*` is configured with:

```text
UACC(NONE)
USER2 READ
```

This demonstrates explicit access assignment.

## Finding 4 — USER2 exists but cannot use TSO

`LISTUSER USER2` confirms that the RACF user exists.

However, the TSO logon attempt returned:

```text
IKJ56420I Userid USER2 not authorized to use TSO
```

So the lab did not perform an interactive `USER2` browse test.

## Finding 5 — Search mask did not return the expected entries

The captured `SEARCH CLASS(DATASET) MASK(IBMUSER.SECLAB*)` did not return entries, but exact `LISTDSD` commands confirmed the profiles.

For future searches, use exact `LISTDSD` checks or a revised mask such as:

```text
SEARCH CLASS(DATASET) MASK(IBMUSER.SECLAB.*)
```
