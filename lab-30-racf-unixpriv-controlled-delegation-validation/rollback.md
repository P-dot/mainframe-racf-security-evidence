# Rollback — Lab 30

## UNIXPRIV rollback performed
```text
PERMIT SUPERUSER.FILESYS.CHANGEPERMS CLASS(UNIXPRIV) ID(H7USER) DELETE
SETROPTS RACLIST(UNIXPRIV) REFRESH
RLIST UNIXPRIV SUPERUSER.FILESYS.CHANGEPERMS ALL
```

The final functional test from H7USER was denied, proving that the delegated capability had been removed.

## Intentional lab identity state retained
The OMVS identity preparation for the dedicated test account was retained:
- H7GRP GID 1000
- H7USER UID 1000
- HOME `/u/h7user`
- PROGRAM `/bin/sh`

These settings are identity-enablement state, not the temporary UNIXPRIV authorization tested by this lab.

## Optional full identity cleanup
Only perform this if the H7USER UNIX identity is no longer needed by later labs. It was **not** part of the executed rollback in this lab, so no unexecuted destructive commands are presented as completed work.
