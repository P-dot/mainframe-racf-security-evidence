# Evidence Mapping

| Audit area | Main evidence source | Key observation |
|---|---|---|
| RACF Health Checker / sensitive resources | Lab 03 | RACF-sensitive resources and APF-related exceptions were reviewed. |
| RACF global model | Lab 04 | `SETROPTS LIST`, `IBMUSER`, `SYS1`, DATASET profile checks established baseline. |
| Started tasks | Lab 05 | SDSF and STARTED class review mapped technical tasks to RACF identities. |
| Technical users | Lab 06 | `START1`, `START2`, `TCPIP`, `FTPD`, `WEBSRV`, `OMVSKERN`, `IBMUSER` reviewed. |
| OMVS / UID(0) | Lab 07 | Multiple technical IDs showed OMVS superuser exposure. |
| Sensitive datasets | Lab 08 | `SYS1.*`, `ADCD.Z111S.*` and critical libraries returned no visible RACF description in evidence. |
| zFS backing datasets | Lab 09 | zFS aggregates were active and selected backing datasets returned no visible RACF profile. |
| SAF controls | Lab 10 | Limited/no visible `BPX.*`, `UNIXPRIV`, `EZB.*` style controls in evidence. |
| Audit / SMF | Lab 11 | SMF SYS1.MAN recording not used; `SMFPRM00` showed `NOACTIVE`. |
| JES2 / SDSF / OPERCMDS | Lab 12 | Broad SDSF visibility; no visible granular profiles for several JES/SDSF classes. |
| APF / PROGRAM / LINKLIST | Lab 13 | PROGRAM profiles present; APF and LINKLIST active; selected libraries lacked visible DATASET profiles. |

## Evidence handling note

The final report references previous labs instead of duplicating every screenshot. The screenshots and source Word documents remain in their respective lab folders.
