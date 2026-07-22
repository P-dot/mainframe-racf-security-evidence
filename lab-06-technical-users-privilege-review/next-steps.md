# Recommended Next Steps

## Follow-up commands

Run these read-only commands to complete the UNIX/security angle:

```text
LISTUSER TCPIP OMVS
LISTUSER FTPD OMVS
LISTUSER WEBSRV OMVS
LISTUSER OMVSKERN OMVS
LISTGRP OMVSGRP
RLIST UNIXPRIV * ALL
RLIST FACILITY BPX.* ALL
```

Do not change anything yet.

## Next lab proposal

`lab-07-zos-unix-technical-ids-omvs-review`

Main objective:

- detect UID/GID information
- identify possible UID(0) or shared UNIX identities
- review UNIXPRIV/FACILITY exposure
- connect OMVS identities with services such as TCPIP, SSHD, INETD, HTTPD and FTPD
