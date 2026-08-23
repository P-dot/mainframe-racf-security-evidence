# Lab 28 — Operations / Panel Path Reference

This lab was intentionally performed primarily through RACF ISPF panels rather than as a command-only exercise.

## RACF panel path

```text
ISPF
 -> RACF SERVICES OPTION MENU
```

### Group profile

```text
3  GROUP PROFILES AND USER-TO-GROUP CONNECTIONS
D  DISPLAY
1  ADD
4  CONNECT
```

Created:

```text
RACFL28
OWNER=IBMUSER
SUPGROUP=SYS1
```

Connection:

```text
USER=IBMUSER
DEFAULT UACC=NONE
GROUP AUTHORITY=USE
GROUP-SPECIAL=NO
GROUP-OPERATIONS=NO
GROUP-AUDITOR=NO
```

### DATASET profile

```text
1  DATA SET PROFILES
S/9 SEARCH
1   ADD
D/8 DISPLAY
4   ACCESS
```

Search criteria used to establish the baseline:

```text
MASK1=RACFL28
TYPE=GENERIC
```

Initial attempted generic name:

```text
RACFL28.**
```

Observed error:

```text
IKJ56702I INVALID DATASET NAME
```

### System options troubleshooting

```text
5 SYSTEM OPTIONS
1 DISPLAY
```

Relevant observed state:

```text
GENERIC PROFILE CLASSES includes DATASET
ENHANCED GENERIC NAMING IS NOT IN EFFECT
```

No global EGN change was made.

Compatible generic profile used:

```text
RACFL28.*
TYPE=GENERIC
OWNER=RACFL28
UACC=NONE
FAILED ACCESSES=FAIL
AUDIT SUCCESSES=NOAUDIT
AUDIT FAILURES=READ
```

Access-list entry:

```text
RACFL28  ALTER
```

### Functional dataset test

Allocated:

```text
IBMUSER.RACFL28.TEST
```

Sequential dataset parameters:

```text
SPACE=TRACK
PRIMARY=5
SECONDARY=2
DIRECTORY BLOCKS=0
RECFM=FB
LRECL=80
BLKSIZE=27920
DATA SET NAME TYPE=BASIC
```

Test record:

```text
RACF LAB 28 DATASET ACCESS TEST
```
