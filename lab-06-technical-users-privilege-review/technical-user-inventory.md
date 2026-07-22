# Technical User Inventory

| User ID | Observed use from prior started-task review | Default group | Global attributes shown | Group connection shown | Key finding |
|---|---|---:|---|---|---|
| `START1` | VTAM / TSO / NFS-related tasks in prior SDSF DA evidence | `SYS1` | `OPERATIONS`, `PROTECTED` | `SYS1` with `AUTH=USE` | Protected technical ID, but `OPERATIONS` is a sensitive privilege. |
| `START2` | SDSF, CICS, DB2, MQ, SSHD, TN3270, PORTMAP in prior SDSF DA evidence | `SYS1` | `PROTECTED` | `SYS1` with `AUTH=USE` | No global special attributes shown, but many critical services concentrate under this ID. |
| `TCPIP` | TCP/IP stack | `OMVSGRP` | `NONE` | `OMVSGRP` with `AUTH=USE` | Specific service ID. No `PROTECTED` attribute shown in captured output. |
| `FTPD` | FTPD1 | `SYS1` | `NONE` | `SYS1` with `AUTH=USE` | Specific FTP service ID. No `PROTECTED` attribute shown in captured output. |
| `WEBSRV` | HTTPD1 | `IMWEB` | `NONE` | `IMWEB` with `AUTH=USE` | Specific web service ID. No `PROTECTED` attribute shown in captured output. |
| `OMVSKERN` | INETD4 / OMVS-related service | `OMVSGRP` | `NONE` | `OMVSGRP` with `AUTH=USE` | z/OS UNIX-related technical ID. No OMVS UID segment is visible in captured output. |
| `IBMUSER` | Lab administration | `SYS1` | `SPECIAL`, `OPERATIONS` | `SYSCTLG`, `VSAMDSET`, `SYS1` with `AUTH=JOIN` | Full lab administrator. Not a production-style least-privilege model. |

## Important observation

No captured `LISTUSER` output shows an `OMVS INFORMATION` segment or `UID=` value. For a deeper UNIX-side review, run targeted commands such as:

```text
LISTUSER TCPIP OMVS
LISTUSER OMVSKERN OMVS
LISTUSER WEBSRV OMVS
```

If RACF says no OMVS information is defined, that is also useful evidence.
