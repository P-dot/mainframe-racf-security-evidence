# OMVS Technical User Inventory

| RACF ID | Default group | RACF attributes visible | OMVS UID | HOME | PROGRAM | Evidence | Assessment |
|---|---:|---|---:|---|---|---|---|
| TCPIP | OMVSGRP | NONE | 0 | `/` | Not visible / not specified in captured output | `LISTUSER TCPIP OMVS` | Network technical ID has superuser UNIX UID. Requires justification in production. |
| OMVSKERN | OMVSGRP | NONE | 0 | `/` | `/bin/sh` | `LISTUSER OMVSKERN OMVS` | UNIX kernel-related technical ID with UID 0. Sensitive but common in lab images. |
| IBMUSER | SYS1 | SPECIAL OPERATIONS | 0 | `/u/ibmuser` | `/bin/sh` | `LISTUSER IBMUSER OMVS` | Lab superuser. Highest risk in production. |
| START1 | SYS1 | OPERATIONS, PROTECTED | 0 | `/` | Not visible / not specified in captured output | `LISTUSER START1 OMVS` | Protected started-task ID, but combines RACF OPERATIONS and UNIX UID 0. |
| START2 | Not captured directly | Not captured directly in this lab | Associated with U0 in UNIXMAP | Not captured | Not captured | `RLIST UNIXMAP U0 ALL` | Needs follow-up with `LISTUSER START2 OMVS`. |
| FTPD | SYS1 | NONE | 0 | `/` | Not visible / not specified in captured output | `LISTUSER FTPD OMVS` | FTP daemon ID has UID 0. Needs justification in production. |
| WEBSRV | IMWEB | NONE | 0 | `/usr/lpp/internet` | `/bin/sh` | `LISTUSER WEBSRV OMVS` | Web server ID has UID 0 and an interactive shell. Needs review in production. |

## Key point

The key security finding is not only that individual users have UID 0. The broader finding is that multiple technical IDs share the same privileged UNIX identity. This reduces individual accountability for z/OS UNIX security checks.
