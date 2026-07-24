# Findings

## Finding 1 — zFS filesystems are active and mounted RDWR

`/D OMVS,F` showed multiple active zFS filesystems mounted in read/write mode. Examples include:

```text
DFH410.ZFS
ACD211.SACDZFS1
DSN910.SJVAZFS
DAH910.ADAHZFS1
DSN910.SDSNWORF
DSN910.SDSNMQS
DSN910.SDSNJCC
```

This is expected in a running ADCD system, but it makes dataset-level protection important because these UNIX filesystems are backed by MVS datasets.

## Finding 2 — Reviewed zFS backing datasets did not show RACF DATASET profiles

Commands:

```text
LISTDSD DA('DFH410.ZFS') ALL
LISTDSD DA('DSN910.SJVAZFS') ALL
LISTDSD DA('DSN910.SDSNGLS') ALL
```

Observed pattern:

```text
ICH35003I NO RACF DESCRIPTION FOUND
```

Interpretation:

No RACF DATASET profile was visible for the reviewed zFS backing datasets.

## Finding 3 — No generic profiles found for DFH410.* or DSN910.*

Commands:

```text
SEARCH CLASS(DATASET) MASK(DFH410.*)
SEARCH CLASS(DATASET) MASK(DSN910.*)
```

Observed pattern:

```text
ICH31005I NO ENTRIES MEET SEARCH CRITERIA
```

Interpretation:

No generic RACF DATASET profiles were found for the checked CICS/DB2 dataset families.

## Finding 4 — No BPX.* FACILITY profiles found

Command:

```text
SEARCH CLASS(FACILITY) MASK(BPX*)
```

Observed:

```text
ICH31005I NO ENTRIES MEET SEARCH CRITERIA
```

Interpretation:

No visible `BPX.*` controls were found in the captured evidence.

## Finding 5 — No UNIXPRIV profiles found

Command:

```text
SEARCH CLASS(UNIXPRIV) MASK(*)
```

Observed:

```text
ICH31005I NO ENTRIES MEET SEARCH CRITERIA
```

Interpretation:

No evidence of granular `UNIXPRIV` privileges was found.

## Finding 6 — No checked EZB.* SERVAUTH profiles found

Commands checked `EZB.NETSTAT*`, `EZB.STACKACCESS*`, `EZB.PORTACCESS*`, and `EZB.PAGENT*`.

Observed pattern:

```text
ICH31005I NO ENTRIES MEET SEARCH CRITERIA
```

Interpretation:

No visible granular TCP/IP SERVAUTH profiles were found for the checked areas.

## Finding 7 — TCPIP.* STARTED profile exists, but no STDATA was visible

`SEARCH CLASS(STARTED) MASK(TCPIP)` found:

```text
TCPIP.* (G)
```

`RLIST STARTED TCPIP.* ALL` showed UACC(NONE), WARNING(NO), and auditing of read failures, but no visible `STDATA` segment.

Interpretation:

There is a STARTED profile for TCPIP, but the captured evidence does not prove runtime identity assignment via STDATA.
