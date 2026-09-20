# Part 2 Findings

## F1 — TSO/E SUBMIT is independently restricted

`H7USER` received `IKJ56251I USER NOT AUTHORIZED FOR SUBMIT`. The `TSOAUTH JCL` profile is defined with `UACC(NONE)` and does not grant H7USER access.

## F2 — FTP/JES submission succeeds for the same RACF identity

The same `H7USER` identity authenticated to FTP, used `SITE FILE=JES`, submitted `FTPJES2`, and received `JOB07465`.

## F3 — Successful execution is proven

JES assigned the job to `H7USER`; the harmless `IEFBR14` step ran and completed with condition code `0000`.

## F4 — Broad RACF privilege does not explain the result

`LISTUSER H7USER` showed `ATTRIBUTES=NONE`.

## F5 — SURROGAT does not explain the execution identity

`IBMUSER.SUBMIT` exists with `UACC(NONE)`, and H7USER is not present in its access list. The spool shows `USERID H7USER IS ASSIGNED TO THIS JOB`.

## F6 — JESJOBS and JESINPUT require careful interpretation

Both classes are active in the RACF configuration captured during the test, while searches returned no matching profiles. The lab records the configuration and observed runtime result without assuming an undocumented decision path.

## F7 — JES2 internal-reader configuration permits batch

`$D INTRDR` showed `BATCH=YES`, `CLASS=A`, `HOLD=NO`, and `SYSAFF=(ANY)`.

## Final finding

The tested system exhibits **channel-dependent authorization behavior**: denial of TSO/E `SUBMIT` does not imply denial of FTP/JES batch submission for the same authenticated RACF identity.
