# Defensive Control

## UNIXPRIV

Existing `UNIXPRIV` profiles were verified with `UACC(NONE)`. This confirms that privileged UNIX resources are not universally exposed.

## TEMPDSN

`TEMPDSN` was activated with:

```text
SETROPTS CLASSACT(TEMPDSN)
```

This moves temporary dataset protection into RACF active-class coverage.

## OPERCMDS

`OPERCMDS` was handled with additional care:

```text
RDEFINE OPERCMDS MVS.** UACC(NONE) WARNING
PERMIT MVS.** CLASS(OPERCMDS) ID(IBMUSER) ACCESS(READ)
SETROPTS CLASSACT(OPERCMDS)
SETROPTS RACLIST(OPERCMDS)
SETROPTS RACLIST(OPERCMDS) REFRESH
```

The `WARNING` state is acceptable here only as a laboratory transition state. It prevents immediate operational lockout while Health Checker validates class activation.

A production system would need a more granular command model rather than a broad `MVS.**` profile.
