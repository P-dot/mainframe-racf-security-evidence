# Commands Used

All commands were read-only.

## RACF global settings

```text
SETROPTS LIST
```

## WARNING profile discovery

```text
SEARCH CLASS(DATASET) WARNING
SEARCH CLASS(FACILITY) WARNING
SEARCH CLASS(STARTED) WARNING
SEARCH CLASS(SERVAUTH) WARNING
```

## AUDIT profile discovery

```text
SEARCH CLASS(DATASET) AUDIT
SEARCH CLASS(SERVAUTH) AUDIT
```

Note: the command syntax returned an `INVALID KEYWORD, AUDIT` message in this environment. The response still displayed matching profile information in the command output area.

## STARTED profile audit review

```text
RLIST STARTED TCPIP.* ALL
RLIST STARTED FTPD.* ALL
RLIST STARTED HZSPROC ALL
```

## SMF display

Run from SDSF LOG / console command line:

```text
/D SMF
/D SMF,O
```

## SMFPRM00 browse

Browse only, no edit:

```text
ADCD.Z111S.PARMLIB(SMFPRM00)
```
