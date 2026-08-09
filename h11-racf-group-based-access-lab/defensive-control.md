# Defensive Control — Group-Based RACF Access

The defensive model is:

```text
UACC(NONE)
+
PERMIT to a RACF group
+
no direct user entry unless justified
```

For this lab:

```text
IBMUSER.SECLAB.GROUP.* -> UACC(NONE)
H7GRP                  -> READ
H7USER                 -> no direct profile entry
```

This supports least privilege and role-based administration.

