# Commands — Lab 30

## IBMUSER — RACF administration

### Validate candidate GID and configure H7GRP
```text
RLIST UNIXMAP G1000 ALL
ALTGROUP H7GRP OMVS(GID(1000))
LISTGRP H7GRP OMVS
```

### Validate candidate UID and configure H7USER
```text
RLIST UNIXMAP U1000 ALL
LISTUSER IBMUSER OMVS
ALTUSER H7USER OMVS(UID(1000) HOME('/u/h7user') PROGRAM('/bin/sh'))
LISTUSER H7USER OMVS
```

### Inspect UNIXPRIV baseline
```text
RLIST UNIXPRIV SUPERUSER.FILESYS.CHANGEPERMS ALL
```

### Grant minimum privilege and refresh RACLIST
```text
PERMIT SUPERUSER.FILESYS.CHANGEPERMS CLASS(UNIXPRIV) ID(H7USER) ACCESS(READ)
SETROPTS RACLIST(UNIXPRIV) REFRESH
RLIST UNIXPRIV SUPERUSER.FILESYS.CHANGEPERMS ALL
```

### Roll back explicit authorization
```text
PERMIT SUPERUSER.FILESYS.CHANGEPERMS CLASS(UNIXPRIV) ID(H7USER) DELETE
SETROPTS RACLIST(UNIXPRIV) REFRESH
RLIST UNIXPRIV SUPERUSER.FILESYS.CHANGEPERMS ALL
```

## IBMUSER — minimal USS preparation
```sh
OMVS
pwd
ls -ld /u
mkdir /u/h7user
ls -ld /u/h7user

cd /tmp
touch lab30_ibmuser.txt
chmod 600 lab30_ibmuser.txt
ls -l lab30_ibmuser.txt
```

## H7USER — identity validation
A fresh login was used after changing the OMVS segment.

```sh
OMVS
id
pwd
ls -ld /u/h7user
```

## H7USER — BEFORE / DENIED
```sh
ls -l /tmp/lab30_ibmuser.txt
chmod 644 /tmp/lab30_ibmuser.txt
ls -l /tmp/lab30_ibmuser.txt
```

## H7USER — AFTER GRANT / ALLOWED
```sh
ls -l /tmp/lab30_ibmuser.txt
chmod 644 /tmp/lab30_ibmuser.txt
ls -l /tmp/lab30_ibmuser.txt
```

## H7USER — AFTER ROLLBACK / DENIED
```sh
ls -l /tmp/lab30_ibmuser.txt
chmod 600 /tmp/lab30_ibmuser.txt
ls -l /tmp/lab30_ibmuser.txt
```
