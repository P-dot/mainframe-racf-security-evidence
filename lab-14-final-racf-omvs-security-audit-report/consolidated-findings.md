# Consolidated Findings

## F-01 — ADCD is functional but not hardened as production

Evidence across the labs repeatedly showed a lab-style baseline:

```text
PROTECT-ALL OPTION IS NOT IN EFFECT
ERASE-ON-SCRATCH IS INACTIVE
AUTOMATIC DATASET PROTECTION IS NOT IN EFFECT
ENHANCED GENERIC NAMING IS NOT IN EFFECT
```

**Impact:** production-style least privilege and full accountability cannot be assumed.

**Recommendation:** treat ADCD as a training system and build controlled hardening exercises before applying any concept to production.

---

## F-02 — High privilege concentration in RACF users and technical IDs

`IBMUSER` was observed as a highly privileged lab user. `START1` and `START2` are central technical IDs used by multiple started tasks. `START1` also showed `OPERATIONS` in prior evidence.

**Impact:** compromise or misuse of one ID may affect many services.

**Recommendation:** in a production model, separate duties, minimize `SPECIAL`/`OPERATIONS`, use protected service IDs, and audit privileged IDs.

---

## F-03 — Multiple technical IDs have or share UID(0)

The OMVS review showed several technical IDs associated with UNIX superuser identity.

**Impact:** multiple IDs may act with UNIX superuser authority, weakening separation and attribution.

**Recommendation:** reduce UID(0), use unique UIDs, prefer granular `UNIXPRIV` profiles where supported, and document all remaining superuser requirements.

---

## F-04 — Sensitive DATASET protection gaps were observed

Several system and product libraries returned:

```text
ICH35003I NO RACF DESCRIPTION FOUND
```

Examples included system libraries, ADCD libraries, zFS backing datasets and APF/LINKLIST-related libraries.

**Impact:** RACF dataset protection coverage is not demonstrated for those resources in the collected evidence.

**Recommendation:** create a dataset protection baseline for system libraries, APF libraries, zFS aggregates, PARMLIB/PROCLIB, and product libraries. Use test profiles first.

---

## F-05 — zFS filesystems are active and mounted RDWR

The zFS review showed active zFS filesystems mounted read/write, including CICS/DB2-related aggregates.

**Impact:** zFS content can affect UNIX services and product runtime behavior. Backing datasets must be protected at RACF DATASET level as well as by UNIX permissions.

**Recommendation:** identify all mounted aggregates, protect backing datasets, review mount options, and map zFS paths to RACF controls.

---

## F-06 — Limited visible granular SAF controls

The audit did not show broad granular use of:

```text
BPX.*
UNIXPRIV
EZB.*
OPERCMDS
JESJOBS
JESSPOOL
SDSF
```

**Impact:** access may depend more on broad user authority than on fine-grained SAF profiles.

**Recommendation:** define a granular SAF control model for OMVS, TCP/IP, JES2, SDSF and operator commands.

---

## F-07 — Audit and SMF evidence are insufficient for production-style accountability

The SMF review showed:

```text
SMF SYS1.MAN RECORDING NOT BEING USED
SMFPRM00: NOACTIVE
```

**Impact:** forensic traceability and audit reporting are limited in this lab configuration.

**Recommendation:** validate SMF recording, RACF logging options, SMF type coverage, retention, and downstream reporting.

---

## F-08 — JES2/SDSF visibility is broad while granular profiles were not found

The JES/SDSF lab showed broad operational visibility from SDSF while searches for several JES/SDSF/OPERCMDS-related profiles returned no matching entries.

**Impact:** in production, JES/SDSF access requires strong controls because it exposes job output, SYSLOG, and operator-level workflows.

**Recommendation:** implement and test `OPERCMDS`, `JESJOBS`, `JESSPOOL`, and `SDSF` controls using least privilege.

---

## F-09 — APF, PROGRAM and LINKLIST require strict protection

PROGRAM profiles were present, APF list was active, and LINKLIST was displayed. Several reviewed libraries did not show visible DATASET profiles.

**Impact:** weak protection of authorized libraries can create integrity and privilege escalation risk.

**Recommendation:** protect all APF and LINKLIST libraries, control dynamic APF commands, review PROGRAM profiles, and audit update access.
