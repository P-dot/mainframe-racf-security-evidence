# Defensive Control — RACF Evidence Interpretation

## Defensive idea

A denied RACF access is useful only if it can be interpreted and correlated.

The key defensive fields are:

```text
USER
GROUP
CLASS
DATASET
ACCESS INTENT
ACCESS ALLOWED
```

## What the lab proves

The lab proves that the same test user can produce different outcomes depending on RACF configuration:

```text
PRIVATE.DATA  -> denied
GRANTED.DATA  -> allowed
AUDIT.DATA    -> denied / prepared for failure auditing
```

## Why this matters

A security analyst must not only know that RACF denied access. They must understand why it denied access and which profile caused the decision.
