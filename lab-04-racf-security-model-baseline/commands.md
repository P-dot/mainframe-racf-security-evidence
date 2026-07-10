# Commands executed

All commands were executed from ISPF option 6 / TSO command shell.

## 1. Inspect RACF user

```text
LISTUSER IBMUSER
```

Purpose: display RACF attributes, default group, group connections, revoke status, and last access information for IBMUSER.

## 2. Inspect primary RACF group

```text
LISTGRP SYS1
```

Purpose: display group ownership, subgroups, connected users, and group-level access information.

## 3. Inspect global RACF options

```text
SETROPTS LIST
```

Purpose: display active RACF classes, generic profile classes, RACLIST status, password policy, and global security options.

## 4. Inspect generic dataset profile for IBMUSER datasets

```text
LISTDSD DA('IBMUSER.*') GENERIC ALL
```

Purpose: check whether a generic RACF DATASET profile protects IBMUSER-owned datasets.

## 5. Inspect profile for an ADCD sensitive library

```text
LISTDSD DA('ADCD.Z111S.LINKLIB') ALL
```

Purpose: verify whether a sensitive executable library has an explicit RACF DATASET profile.

## 6. Inspect STARTED class profiles

```text
RLIST STARTED * ALL
```

Purpose: list STARTED class profiles and confirm started task profile visibility. The output is long; for future work, targeted RLIST commands should be used instead of wildcard listing.

Recommended targeted follow-up commands:

```text
SEARCH CLASS(STARTED) MASK(HZSPROC)
RLIST STARTED HZSPROC ALL
RLIST STARTED HZSPROC.* ALL
```
