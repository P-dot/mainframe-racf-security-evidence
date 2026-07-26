# Findings

## Finding 1 — RACF PROGRAM profiles exist

`SEARCH CLASS(PROGRAM) MASK(*)` returned visible profiles including several `BPX*` entries and `RLOGIND`.

Impact:

```text
The PROGRAM class is populated and can be reviewed further for execution-control posture.
```

## Finding 2 — APF-authorized libraries are active

`/D PROG,APF` displayed an active APF list containing system, middleware, TCP/IP, and runtime libraries.

Impact:

```text
APF-authorized code paths exist and must be protected by strict dataset controls and change governance.
```

## Finding 3 — LINKLIST is active and includes sensitive libraries

`/D PROG,LNKLST` displayed an active LINKLIST set including core system and runtime libraries.

Impact:

```text
LINKLIST libraries are important executable search-path resources and should be protected as sensitive system libraries.
```

## Finding 4 — Several reviewed libraries show no visible RACF DATASET profile

Captured `LISTDSD` outputs show `NO RACF DESCRIPTION FOUND` for multiple selected libraries, including `SYS1.SVCLIB`, `SYS1.LPALIB`, `ADCD.Z111S.LINKLIB`, `CEE.SCLBDLL`, and `CEE.SCEERUN`.

Impact:

```text
In a hardened production system, APF/LINKLIST/runtime libraries should be explicitly protected and audited.
```

## Finding 5 — SYS1 generic dataset profile search returned no entries

`SEARCH CLASS(DATASET) MASK(SYS1.*)` returned no matching profiles in the captured evidence.

Impact:

```text
No visible generic RACF DATASET profile was found for SYS1.* during this check.
```

## Finding 6 — LPA display was not captured successfully

The attempted LPA display returned a syntax error, so no LPA inventory is claimed from this evidence set.

Impact:

```text
LPA review remains optional follow-up evidence, not a completed finding in this lab.
```
