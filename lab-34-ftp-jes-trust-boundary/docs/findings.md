# Findings

1. TCP/21 was observed in LISTEN state under FTPD1 in the tested system.
2. FTPD1 was correlated with TCP/IP profile configuration.
3. H7USER successfully authenticated to the FTP server.
4. Runtime FTP status reported `JESINTERFACELEVEL is 1`.
5. `SITE FILE=JES` was accepted.
6. JES-oriented FTP interaction was reachable from the authenticated session.
7. A controlled JCL stream was transferred through FTP.
8. FTP reported handoff to the JES internal reader and returned `JOB07414` in the captured run.
9. The captured `ISF024I ... NOT AUTHORIZED TO SDSF, NO GROUP ASSIGNMENT` message is an SDSF authorization result, not proof of FTP/JES submission rejection.
10. The captured job later showed JCL-processing failure (`JCL ERROR 312` / `IEF650I UNIDENTIFIED OPERATION FIELD`).
11. Therefore Part 1 proves the ingress path but does not yet prove successful workload execution or isolate the downstream RACF/JES authorization boundary.
12. No privilege was added to force a positive result.

## Evidence quality

The strongest evidence is the ordered chain of runtime NETSTAT state, authenticated FTP responses, effective `STAT` output, `SITE FILE=JES`, internal-reader acknowledgement, JES job identifier, SDSF authorization evidence, and job/JCL diagnostics.

The lab deliberately preserves conflicting-looking outcomes because they belong to different control planes.
