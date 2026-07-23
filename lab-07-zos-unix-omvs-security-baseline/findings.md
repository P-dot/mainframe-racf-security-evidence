# Findings

## Finding 1 — OMVSGRP exists but no GID is visible in the captured output

`LISTGRP OMVSGRP` shows:

```text
GROUP=OMVSGRP
SUPERIOR GROUP=SYS1
OWNER=IBMUSER
USERS: OMVSKERN, TCPIP
```

The capture does not show an OMVS group segment or a visible `GID=` value.

## Finding 2 — Multiple technical IDs have UID 0

The submitted `LISTUSER ... OMVS` evidence shows UID 0 for:

```text
TCPIP
OMVSKERN
IBMUSER
START1
FTPD
WEBSRV
```

This is the main security finding of the lab.

## Finding 3 — START2 needs direct OMVS follow-up

`START2` is not shown in a direct `LISTUSER START2 OMVS` capture, but it appears in the `UNIXMAP U0` profile output. This should be confirmed with:

```text
LISTUSER START2 OMVS
```

## Finding 4 — UNIXPRIV is not used in the visible evidence

`SEARCH CLASS(UNIXPRIV) MASK(*)` returned no entries.

This means the lab evidence does not show granular UNIXPRIV-based delegation of superuser privileges.

## Finding 5 — BPX.* FACILITY profiles were not found

`SEARCH CLASS(FACILITY) MASK(BPX*)` returned no entries.

The evidence does not show BPX.SUPERUSER, BPX.DEFAULT.USER, BPX.UNIQUE.USER, BPX.NEXT.USER, BPX.SERVER or BPX.DAEMON profiles.

## Finding 6 — UID search requires AIM

`SEARCH CLASS(USER) UID(0)` failed with:

```text
ICH31028I The UID keyword requires application identity mapping to be implemented.
```

This is an important operational lesson: in this ADCD system, the correct practical fallback is `RLIST UNIXMAP U0 ALL`.
