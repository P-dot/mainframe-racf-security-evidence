# Findings

## HZSPROC

Command:

```text
RLIST STARTED HZSPROC ALL
```

Observed:

```text
CLASS=STARTED
NAME=HZSPROC
OWNER=IBMUSER
UACC=NONE
YOUR ACCESS=ALTER
WARNING=NO
AUDITING=FAILURES(READ)
USER IBMUSER ACCESS ALTER
```

No visible `STDATA` segment was captured.

Interpretation:

- The profile exists.
- The profile protects or represents the started task name.
- The captured output does not prove that this profile assigns the runtime user/group.

## FTPD

Command:

```text
RLIST STARTED FTPD.* ALL
```

Observed:

```text
CLASS=STARTED
NAME=FTPD.* (G)
OWNER=IBMUSER
UACC=NONE
YOUR ACCESS=ALTER
WARNING=NO
AUDITING=FAILURES(READ)
USER IBMUSER ACCESS ALTER
```

No visible `STDATA` segment was captured.

Interpretation:

- A generic STARTED profile exists for FTPD.
- Runtime SDSF `DA` shows `FTPD1` running with owner `FTPD`.
- The captured RACF profile does not show the `STDATA` source of that assignment.

## TCPIP

Initial incorrect command:

```text
RLIST STARTED TCPIP.** ALL
```

Result:

```text
ICH31053I TCPIP.** NOT FOUND
```

Corrected command:

```text
RLIST STARTED TCPIP.* ALL
```

Observed:

```text
CLASS=STARTED
NAME=TCPIP.* (G)
OWNER=IBMUSER
UACC=NONE
YOUR ACCESS=ALTER
WARNING=NO
AUDITING=FAILURES(READ)
USER IBMUSER ACCESS ALTER
```

No visible `STDATA` segment was captured.

Interpretation:

- The correct generic profile is `TCPIP.*`.
- SDSF `DA` shows the active `TCPIP` task running with owner `TCPIP`.
- The runtime owner is specific and appropriate for a TCP/IP service, but the captured STARTED profile does not show the assignment source.

## Searches with no entries

Commands returned `ICH31005I NO ENTRIES MEET SEARCH CRITERIA` for several patterns, including:

```text
SDSF
SSHD
SSHD4
SSHD*
DB9
DB9*
CICS
CICSA
CICS*
```

Interpretation:

- No matching STARTED profiles were found with those masks.
- This does not prove the services are absent.
- SDSF `DA` confirms that several of these tasks are active.
- The mapping may be through generic defaults, ICHRIN03, or another naming convention not found by the searched masks.
