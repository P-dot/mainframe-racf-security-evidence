# Findings

## Finding 1 — `IBMUSER` is a full-power lab administrator

`LISTUSER IBMUSER` shows:

- `DEFAULT-GROUP=SYS1`
- `ATTRIBUTES=SPECIAL OPERATIONS`
- `AUTH=JOIN` in `SYS1`
- additional `AUTH=JOIN` connections to `SYSCTLG` and `VSAMDSET`

This is normal for ADCD lab administration, but it is not a least-privilege model.

Risk level: **High in production / Expected in lab**

## Finding 2 — `START1` is protected but has `OPERATIONS`

`LISTUSER START1` shows:

- `DEFAULT-GROUP=SYS1`
- `ATTRIBUTES=OPERATIONS`
- `ATTRIBUTES=PROTECTED`
- `GROUP=SYS1 AUTH=USE`

`PROTECTED` is positive because it marks the ID as a technical/non-human ID. However, `OPERATIONS` is powerful and should be justified.

Risk level: **Medium/High**

## Finding 3 — `START2` is protected but heavily used

`LISTUSER START2` shows:

- `DEFAULT-GROUP=SYS1`
- `ATTRIBUTES=PROTECTED`
- `GROUP=SYS1 AUTH=USE`
- no `SPECIAL`, `OPERATIONS`, or `AUDITOR` shown

This is better than `START1` from an attributes point of view. However, the previous started-task inventory showed that many critical tasks run as `START2`.

Risk level: **Medium**

## Finding 4 — `TCPIP` is a dedicated service ID but is not shown as protected

`LISTUSER TCPIP` shows:

- `DEFAULT-GROUP=OMVSGRP`
- `ATTRIBUTES=NONE`
- `GROUP=OMVSGRP AUTH=USE`
- no `PROTECTED` attribute shown

This is good because there are no global RACF powers visible. The missing `PROTECTED` attribute should be reviewed.

Risk level: **Low/Medium**

## Finding 5 — `FTPD` is a dedicated service ID in `SYS1`

`LISTUSER FTPD` shows:

- `DEFAULT-GROUP=SYS1`
- `ATTRIBUTES=NONE`
- `GROUP=SYS1 AUTH=USE`
- no `PROTECTED` attribute shown

The account is not globally privileged, but it is connected to the central `SYS1` group.

Risk level: **Medium**

## Finding 6 — `WEBSRV` is a dedicated web service ID

`LISTUSER WEBSRV` shows:

- `DEFAULT-GROUP=IMWEB`
- `ATTRIBUTES=NONE`
- `GROUP=IMWEB AUTH=USE`
- no `PROTECTED` attribute shown

This is a reasonable separation pattern for the HTTP server, but the account should be checked for interactive logon exposure.

Risk level: **Low/Medium**

## Finding 7 — `OMVSKERN` is z/OS UNIX-related and needs a targeted OMVS review

`LISTUSER OMVSKERN` shows:

- `DEFAULT-GROUP=OMVSGRP`
- `ATTRIBUTES=NONE`
- `GROUP=OMVSGRP AUTH=USE`
- no `OMVS INFORMATION` segment visible in the captured output

Because this ID is related to OMVS/INETD processing, the next review should explicitly check OMVS information and UNIX privileges.

Risk level: **Medium pending OMVS follow-up**

## Finding 8 — `SYS1` concentrates many technical IDs

`LISTGRP SYS1` shows the central group `SYS1`, owned by `IBMUSER`, with many technical/system IDs connected. `START1`, `START2`, and `FTPD` are visible as `AUTH=USE` members.

Risk level: **Medium in lab / High if unmanaged in production**
