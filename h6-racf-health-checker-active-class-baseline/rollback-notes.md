# Rollback Notes

Rollback should be used only if console or SDSF commands start failing unexpectedly.

## OPERCMDS rollback

```text
SETROPTS NORACLIST(OPERCMDS)
SETROPTS NOCLASSACT(OPERCMDS)
```

If the broad laboratory profile must be removed:

```text
RDELETE OPERCMDS MVS.**
SETROPTS RACLIST(OPERCMDS) REFRESH
```

## TEMPDSN rollback

Only if temporary dataset processing breaks:

```text
SETROPTS NOCLASSACT(TEMPDSN)
```

## UNIXPRIV rollback

Avoid unless there is a serious emergency, because this class was already part of prior hardening work.

```text
SETROPTS NORACLIST(UNIXPRIV)
SETROPTS NOCLASSACT(UNIXPRIV)
```

## Safer operational rule

Do not roll back working hardening just because `RACF_SENSITIVE_RESOURCES` still shows an exception. That check is out of scope for this lab.
