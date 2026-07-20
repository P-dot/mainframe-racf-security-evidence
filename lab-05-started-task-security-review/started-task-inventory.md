# Started Task Runtime Inventory

The following inventory is based on the SDSF `DA` screenshots captured during the lab.

| Started task / job | Runtime owner observed | Notes |
|---|---:|---|
| VTAM | START1 | Core networking/SNA service using generic technical user. |
| RACF | START2 | Security product address space using generic technical user. |
| TSO | START1 | TSO service using generic technical user. |
| HTTPD1 | WEBSRV | Uses a service-specific user. |
| SDSF | START2 | SDSF running under generic technical user. |
| CICSA | START2 | CICS region using generic technical user. |
| TCPIP | TCPIP | Uses a service-specific user. |
| TN3270 | START2 | TN3270 using generic technical user. |
| DB9GMSTR | START2 | DB2 master address space using generic technical user. |
| DB9DIST | START2 | DB2 distributed address space using generic technical user. |
| DB9IRLM | START2 | DB2 IRLM using generic technical user. |
| CSQ7MSTR | START2 | MQ queue manager using generic technical user. |
| FTPD1 | FTPD | Uses a service-specific user. |
| INETD4 | OMVSKERN | z/OS UNIX network daemon area using OMVS-related user. |
| SSHD4 | START2 | SSH daemon using generic technical user. |
| PORTMAP | START2 | Portmap using generic technical user. |
| IBMUSER | IBMUSER | Interactive TSO user/session. |

## Main observation

The ADCD environment uses a mixture of:

- Service-specific technical users: `TCPIP`, `FTPD`, `WEBSRV`, `OMVSKERN`.
- Generic privileged service users: `START1`, `START2`.

This is expected in a training image but would require detailed review in a production hardening project.
