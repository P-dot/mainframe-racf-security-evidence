# Defensive control

The defensive pattern is:

```text
UACC(NONE)
+
PERMIT only the required user with only the required access
+
Remove the permission when the exception ends
```

The lab does not change `UACC` to `READ`. The resource remains closed by default throughout the exercise.

Temporary access is granted only to:

```text
ID(H7USER) ACCESS(READ)
```

and later removed using:

```text
PERMIT 'IBMUSER.SECLAB.PRIVATE.*' CLASS(DATASET) ID(H7USER) DELETE
```
