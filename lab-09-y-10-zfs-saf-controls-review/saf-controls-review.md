# SAF controls review — FACILITY, UNIXPRIV, SERVAUTH and STARTED

## SETROPTS LIST

`SETROPTS LIST` was captured to document active RACF classes and RACLIST status.

Relevant classes for this lab:

- `FACILITY`
- `UNIXPRIV`
- `SERVAUTH`
- `STARTED`
- `DATASET`

The exact enabled/RACLISTed state should be read from the screenshot evidence. The important point for the lab is that searches were performed against these SAF classes to determine whether granular controls are implemented.

## FACILITY BPX.*

Command:

```text
SEARCH CLASS(FACILITY) MASK(BPX*)
```

Observed result:

```text
ICH31005I NO ENTRIES MEET SEARCH CRITERIA
```

Interpretation:

No visible `BPX.*` profiles were found in class `FACILITY` using this search. That means no evidence was found for granular BPX controls such as:

```text
BPX.SUPERUSER
BPX.DAEMON
BPX.SERVER
BPX.DEFAULT.USER
BPX.UNIQUE.USER
BPX.FILEATTR.APF
BPX.FILEATTR.PROGCTL
BPX.FILEATTR.SHARELIB
```

## UNIXPRIV

Command:

```text
SEARCH CLASS(UNIXPRIV) MASK(*)
```

Observed result:

```text
ICH31005I NO ENTRIES MEET SEARCH CRITERIA
```

Interpretation:

No visible `UNIXPRIV` profiles were found. This matters because `UNIXPRIV` is the more granular way to grant selected z/OS UNIX privileges instead of relying broadly on `UID(0)`.

## SERVAUTH / EZB.*

Commands:

```text
SEARCH CLASS(SERVAUTH) MASK(EZB*)
SEARCH CLASS(SERVAUTH) MASK(EZB.NETSTAT*)
SEARCH CLASS(SERVAUTH) MASK(EZB.STACKACCESS*)
SEARCH CLASS(SERVAUTH) MASK(EZB.PORTACCESS*)
SEARCH CLASS(SERVAUTH) MASK(EZB.PAGENT*)
```

Observed result pattern:

```text
ICH31005I NO ENTRIES MEET SEARCH CRITERIA
```

Interpretation:

No visible `EZB.*` TCP/IP SERVAUTH profiles were found with these searches. This suggests the lab system is not using granular SERVAUTH profiles for the checked TCP/IP controls.

## STARTED class cross-check

Commands:

```text
SEARCH CLASS(STARTED) MASK(TCPIP)
RLIST STARTED TCPIP.* ALL
SEARCH CLASS(STARTED) MASK(FTPD*)
SEARCH CLASS(STARTED) MASK(SSHD*)
SEARCH CLASS(STARTED) MASK(INETD*)
```

Observed:

```text
TCPIP.* (G)
```

`RLIST STARTED TCPIP.* ALL` showed:

```text
CLASS = STARTED
NAME = TCPIP.* (G)
OWNER = IBMUSER
UACC = NONE
WARNING = NO
AUDITING = FAILURES(READ)
USER IBMUSER ACCESS ALTER
```

No `STDATA` segment was visible in the captured output. Therefore the profile exists, but the evidence does not prove assignment of runtime user/group through `STDATA`.

For `FTPD*`, `SSHD*`, and `INETD*`, the captured result was:

```text
ICH31005I NO ENTRIES MEET SEARCH CRITERIA
```
