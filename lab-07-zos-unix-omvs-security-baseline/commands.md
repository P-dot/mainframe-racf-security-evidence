# Commands Used

All commands were executed from ISPF option 6 as read-only RACF review commands.

```text
LISTGRP OMVSGRP
LISTUSER TCPIP OMVS
LISTUSER OMVSKERN OMVS
LISTUSER IBMUSER OMVS
LISTUSER START1 OMVS
LISTUSER START2 OMVS
LISTUSER FTPD OMVS
LISTUSER WEBSRV OMVS
SEARCH CLASS(UNIXPRIV) MASK(*)
SEARCH CLASS(FACILITY) MASK(BPX*)
SEARCH CLASS(USER) UID(0)
RLIST UNIXMAP U0 ALL
```

## Notes

`LISTUSER START2 OMVS` was requested but is not visible as an individual LISTUSER output in the submitted evidence. However, `START2` appears in the `UNIXMAP U0` profile output, so it should be followed up with a direct `LISTUSER START2 OMVS` capture.

No commands such as `ALTUSER`, `RDEFINE`, `RALTER`, `PERMIT`, `SETROPTS`, or `CONNECT` were executed as part of this lab.
