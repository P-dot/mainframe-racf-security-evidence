# Lab 07 — z/OS UNIX / OMVS Security Baseline

## Purpose

This lab reviews the z/OS UNIX / OMVS security posture of selected RACF technical users in the ADCD z/OS 1.11 training environment.

The review is read-only. No RACF or OMVS security definitions were modified.

## Scope

Reviewed evidence:

- `LISTGRP OMVSGRP`
- `LISTUSER TCPIP OMVS`
- `LISTUSER OMVSKERN OMVS`
- `LISTUSER IBMUSER OMVS`
- `LISTUSER START1 OMVS`
- `LISTUSER FTPD OMVS`
- `LISTUSER WEBSRV OMVS`
- `SEARCH CLASS(UNIXPRIV) MASK(*)`
- `SEARCH CLASS(FACILITY) MASK(BPX*)`
- `SEARCH CLASS(USER) UID(0)`
- `RLIST UNIXMAP U0 ALL`

## Main result

Several technical users have `UID(0)`, including TCPIP, OMVSKERN, IBMUSER, START1, FTPD and WEBSRV. The `UNIXMAP U0` profile also lists additional IDs associated with UID 0, including START2, BPXOINIT and INETD.

No UNIXPRIV profiles were found with `SEARCH CLASS(UNIXPRIV) MASK(*)`, and no BPX* FACILITY profiles were found with `SEARCH CLASS(FACILITY) MASK(BPX*)`.

## Evidence

The original Word document is preserved in:

```text
/evidence/source-documents/LISTUSER.docx
```

Extracted screenshots are stored in:

```text
/evidence/screenshots/
```

## Professional interpretation

This is normal for an ADCD training image, but it is not a hardened production model. In production, the number of users with UID 0 should be minimized, and granular z/OS UNIX privileges should be reviewed through UNIXPRIV and BPX.* FACILITY controls where applicable.
