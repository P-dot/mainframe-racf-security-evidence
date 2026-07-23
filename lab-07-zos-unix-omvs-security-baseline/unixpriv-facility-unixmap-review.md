# UNIXPRIV, FACILITY BPX and UNIXMAP Review

## UNIXPRIV

Command:

```text
SEARCH CLASS(UNIXPRIV) MASK(*)
```

Result:

```text
ICH31005I NO ENTRIES MEET SEARCH CRITERIA
```

Interpretation:

No UNIXPRIV profiles were found in the submitted evidence. This means the reviewed environment does not show granular RACF UNIXPRIV controls for z/OS UNIX superuser functions.

## FACILITY BPX.*

Command:

```text
SEARCH CLASS(FACILITY) MASK(BPX*)
```

Result:

```text
ICH31005I NO ENTRIES MEET SEARCH CRITERIA
```

Interpretation:

No BPX* FACILITY profiles were found in the submitted evidence. This includes profiles such as `BPX.SUPERUSER`, `BPX.DEFAULT.USER`, `BPX.UNIQUE.USER`, `BPX.NEXT.USER`, `BPX.SERVER`, or `BPX.DAEMON`.

## SEARCH CLASS(USER) UID(0)

Command:

```text
SEARCH CLASS(USER) UID(0)
```

Result:

```text
ICH31028I The UID keyword requires application identity mapping to be implemented.
```

Interpretation:

The direct UID search is not available in this environment because AIM support is not implemented at the required level. The correct fallback in this lab is the `UNIXMAP` profile review.

## UNIXMAP U0

Command:

```text
RLIST UNIXMAP U0 ALL
```

Result summary:

The `UNIXMAP U0` profile exists and includes multiple IDs in the profile listing, including:

```text
IBMUSER
ADCDMST
BPXOINIT
INETD
FTPD
TCPIP
WEBSRV
DSN1WLM1
START2
START1
OPEN3
OPEN2
OPEN1
```

Interpretation:

This profile is the key evidence that multiple RACF IDs are associated with UID 0. The `ACCESS(NONE)` values in the output should not be read as ordinary resource access grants for operational use; UNIXMAP profiles are RACF-maintained mapping cross-references.
