# Commands Used

## 1. Review exact HZSPROC profile

```text
RLIST STARTED HZSPROC ALL
```

Purpose: verify whether a profile exists for the Health Checker started task and whether it contains `STDATA`.

## 2. Search targeted STARTED profiles

```text
SEARCH CLASS(STARTED) MASK(SDSF)
SEARCH CLASS(STARTED) MASK(TCPIP)
SEARCH CLASS(STARTED) MASK(SSHD)
SEARCH CLASS(STARTED) MASK(FTPD)
SEARCH CLASS(STARTED) MASK(CICS)
SEARCH CLASS(STARTED) MASK(DB)
SEARCH CLASS(STARTED) MASK(SSHD4)
SEARCH CLASS(STARTED) MASK(SSHD*)
SEARCH CLASS(STARTED) MASK(DB9)
SEARCH CLASS(STARTED) MASK(DB9*)
SEARCH CLASS(STARTED) MASK(CICSA)
SEARCH CLASS(STARTED) MASK(CICS*)
```

Purpose: avoid a noisy `RLIST STARTED * ALL` and look for specific service profiles.

## 3. Review FTPD generic profile

```text
RLIST STARTED FTPD.* ALL
```

Purpose: inspect the generic profile found by the search.

## 4. Review TCPIP generic profile

```text
RLIST STARTED TCPIP.* ALL
```

Purpose: inspect the generic profile found by the search. `TCPIP.**` was not valid in this environment because enhanced generic naming is not active.

## 5. Runtime validation in SDSF

```text
SDSF
DA
PREFIX *
OWNER *
```

Purpose: verify the actual runtime owner of active started tasks.

## 6. Optional SYSLOG search

```text
SDSF LOG
F IEF695I
F HZSPROC
F SDSF
F TCPIP
```

Purpose: look for messages that show how a started task was assigned to a RACF user at start time.
