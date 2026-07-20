# Next Steps

## Immediate safe follow-up

1. Locate the started task identity assignment source for tasks without visible `STDATA`.
2. Check whether ICHRIN03 or another fallback mechanism is used.
3. Review `START1` and `START2` users with `LISTUSER`.
4. Review whether `START1` or `START2` have broad dataset or resource access.
5. Map each critical subsystem to an intended service account.

## Candidate commands for the next lab

```text
LISTUSER START1
LISTUSER START2
LISTUSER TCPIP
LISTUSER FTPD
LISTUSER WEBSRV
LISTUSER OMVSKERN
```

```text
LISTGRP <default-group-from-each-user>
```

```text
RLIST STARTED HZSPROC ALL
RLIST STARTED FTPD.* ALL
RLIST STARTED TCPIP.* ALL
```

## Do not execute yet without a rollback plan

```text
RDEFINE STARTED ... STDATA(...)
RALTER STARTED ... STDATA(...)
SETROPTS RACLIST(STARTED) REFRESH
```
