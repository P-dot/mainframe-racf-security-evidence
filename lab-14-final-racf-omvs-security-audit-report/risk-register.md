# Risk Register

| ID | Risk | Severity | Evidence theme | Production impact | Recommended direction |
|---|---|---:|---|---|---|
| R-01 | Privileged lab ID concentration | High | `IBMUSER`, `START1`, `START2` | Broad compromise impact | Reduce/admin-separate privileged IDs |
| R-02 | Repeated UID(0) / UNIX superuser exposure | High | OMVS/UNIXMAP evidence | Weak separation and accountability | Unique UIDs, reduce UID(0), use UNIXPRIV |
| R-03 | Sensitive DATASET profiles not visible | High | `NO RACF DESCRIPTION FOUND` | Unauthorized read/update risk | Define dataset protection baseline |
| R-04 | zFS backing datasets not visibly protected | High | zFS aggregate LISTDSD results | UNIX filesystem integrity risk | Protect zFS aggregates and review mounts |
| R-05 | Limited BPX/UNIXPRIV/EZB controls | Medium/High | SAF class searches | Overreliance on broad IDs | Implement granular SAF controls |
| R-06 | SMF SYS1.MAN recording not active/used | High | `/D SMF`, `SMFPRM00` | Poor forensics/accountability | Validate SMF recording and RACF logging |
| R-07 | JES/SDSF control profiles absent in evidence | Medium/High | OPERCMDS/JESJOBS/JESSPOOL/SDSF searches | Job/spool/operator exposure | Lock down JES/SDSF with SAF profiles |
| R-08 | APF/LINKLIST library protection gaps | High | APF/LINKLIST + LISTDSD evidence | Authorized code escalation risk | Protect APF/LINKLIST and SETPROG controls |
| R-09 | Lab configuration differs from production | Medium | ADCD baseline | Misleading assumptions | Document limitations and avoid production claims |
