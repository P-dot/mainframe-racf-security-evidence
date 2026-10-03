# RACF Security Evidence — Ecosystem Integration

## Purpose

This document defines how `mainframe-racf-security-evidence` participates in the wider IBM z/OS engineering portfolio.

The repository is the cross-cutting **identity, authorization, least-privilege, security-assurance and trust-boundary layer**. It consumes subsystem context from other repositories and owns the RACF/SAF interpretation of that context.

Navigation:

- [Repository README](../README.md)
- [Portfolio](https://github.com/P-dot/P-dot)
- [Architecture V2](https://github.com/P-dot/zos-adcd-hercules-engineering-lab/tree/main/docs/architecture/v2)
- [Engineering Control](https://github.com/P-dot/zos-adcd-hercules-engineering-lab/tree/main/docs/engineering-control)
- [Recruiter Summary](recruiter-summary.md)

---

## 1. Architectural role

RACF is not an isolated subsystem in this portfolio. SAF requests can originate from many z/OS components.

```text
                         protected action
                               |
                               v
                              SAF
                               |
                               v
                             RACF
                               |
                +--------------+--------------+
                |              |              |
                v              v              v
             identity       resource       policy
                \              |              /
                 +-------------+-------------+
                               |
                               v
                    effective authorization
                               |
                               v
                           evidence
```

The repository therefore asks:

```text
WHO
  -> can perform WHAT
  -> against WHICH RESOURCE
  -> under WHICH RACF/SAF STATE
  -> with WHICH EFFECTIVE RESULT
  -> supported by WHICH EVIDENCE
  -> and with WHICH ROLLBACK path?
```

---

## 2. Repository ownership

### Owned here

This repository owns evidence and interpretation around:

- RACF users and groups;
- privileged attributes;
- dataset profiles, UACC and PERMIT;
- RACF classes and SAF general resources;
- FACILITY and OPERCMDS;
- effective-authority analysis;
- controlled test identities;
- functional allow/deny validation;
- audit and violation evidence;
- least-privilege delegation;
- RACLIST effective-authority behavior;
- rollback and rollback validation;
- OMVS identity, UID/GID, UNIXMAP and UNIXPRIV from the RACF side;
- started-task and technical-identity security;
- RACF digital certificates and key rings;
- `IRR.DIGTCERT.*` authorization;
- RACDCERT delegation;
- security interpretation of cross-domain trust boundaries.

### Not owned here

| Technology | Owning repository / domain |
|---|---|
| OMVS shell, POSIX processes, USS filesystem operations | [`UNIX_System_Services-`](https://github.com/P-dot/UNIX_System_Services-) |
| TCP/IP, FTP, TN3270, Policy Agent, AT-TLS | [`zos-communications-server-network-lab`](https://github.com/P-dot/zos-communications-server-network-lab) |
| JES2 execution, spool and core system engineering | [`zos-adcd-hercules-engineering-lab`](https://github.com/P-dot/zos-adcd-hercules-engineering-lab) |
| Scheduling semantics and workload automation | [`zos-batch-scheduler`](https://github.com/P-dot/zos-batch-scheduler) |
| Db2 subsystem and SQL engineering | [`DB2-`](https://github.com/P-dot/DB2-) |
| CICS resource and transaction engineering | [`CICS`](https://github.com/P-dot/CICS) |

Cross-domain evidence does not transfer subsystem ownership to this repository.

---

## 3. Evidence-state vocabulary

Architecture V2 requires explicit separation between demonstrated work and architectural intent.

| State | Definition |
|---|---|
| **VALIDATED LOCALLY** | Evidence is present in this repository |
| **VALIDATED IN TARGET REPOSITORY** | Evidence exists in the repository that owns the target technology |
| **CROSS-DOMAIN / REQUIRES EVIDENCE** | The relationship is identified but the complete path has not been demonstrated |
| **PLANNED** | Future work only |
| **BLOCKER DOCUMENTED** | A real execution boundary prevented continuation and was isolated with evidence |

The following are deliberately not treated as equivalent:

```text
profile exists
      !=
effective access

command succeeded
      !=
functional authorization proof

object exists
      !=
end-to-end integration

architecture documented
      !=
capability validated
```

---

## 4. Evidence tracks

The repository contains two historical lab sequences, but Architecture V2 groups their evidence by engineering purpose.

### Access-control lifecycle — H1-H13

The H-series establishes the foundational lifecycle:

```text
profile
 -> exposure
 -> authorization
 -> controlled identity
 -> allow / deny
 -> evidence
 -> remediation
 -> cleanup
 -> rollback
```

It covers safe dataset profiles, UACC, PERMIT, WARNING, auditing, controlled identities, functional access, violations, temporary authorization, group-based access and rollback.

### RACF / SAF baseline — Labs 02-14

The main series expands into:

- installation-level RACF options;
- Health Checker findings;
- started-task and technical-user security;
- OMVS identity;
- dataset and zFS protection;
- SAF controls;
- audit/accountability;
- JES2/SDSF/OPERCMDS authority;
- APF/PROGRAM/LINKLIST exposure;
- consolidated security review.

### Effective authority — Labs 25-30

This sequence shifts the emphasis from configuration inventory to runtime security behavior.

```text
Lab 25  inventory
   |
Lab 26  effective authority
   |
Lab 27  controlled hardening
   |
Lab 28  functional group/dataset access
   |
Lab 29  UNIXPRIV / UNIXMAP baseline
   |
Lab 30  minimum delegation + rollback proof
```

### Cryptographic trust — Labs 31-33

```text
Lab 31
IRR.DIGTCERT.* / RACDCERT authorization baseline
        |
        v
Lab 32
function-specific LISTRING / ADDRING / DELRING delegation
        |
        v
Lab 33
controlled GENCERT / ring / CONNECT lifecycle
```

Lab 33 deliberately retains:

```text
LAB33CERT
+ RACF-managed private key
+ LAB33RING
+ certificate-to-ring association
```

while removing the temporary administrative delegation used for the experiment.

This is a **RACF-side integration artifact**, not evidence of an active TLS service.

### Trust boundaries — Labs 34-37

The current advanced track applies RACF/SAF reasoning across subsystem boundaries.

```text
Lab 34  FTP -> JES
Lab 35  TN3270 -> TSO authentication surface
Lab 36  TN3270 transport
Lab 37  OMVS / NC110 readiness -> network blocker
```

These labs intentionally distinguish the security boundary being analyzed from the subsystem that owns the underlying technology.

---

## 5. USS integration

### RACF side

RACF owns the identity and authorization view:

```text
RACF user
   |
   +--> OMVS segment
   +--> UID / GID
   +--> UNIXMAP
   +--> UNIXPRIV
   |
   v
effective UNIX authorization
```

Labs 07, 29 and 30 provide relevant local evidence.

### USS side

The USS repository owns:

- shell behavior;
- POSIX processes;
- pathnames;
- file ownership and mode bits;
- filesystem operations;
- shell scripting.

### Current integration state

RACF-to-USS identity and privileged-function behavior is **VALIDATED LOCALLY** where the RACF labs exercise those boundaries.

A broader end-to-end automation path such as:

```text
JCL -> BPXBATCH -> USS script -> RACF-controlled resource
```

requires separate cross-repository evidence.

---

## 6. JES2 / SDSF integration

RACF-side concerns include:

- OPERCMDS;
- JES-related general-resource controls;
- SDSF authorization;
- operator identity;
- started-task identity;
- dataset access;
- channel-specific authorization.

JES2 owns job execution and spool behavior.

### Lab 34: why boundary classification matters

Lab 34 demonstrates that these stages cannot be collapsed into one result:

```text
FTP authentication
       |
       v
FTP/JES ingress
       |
       v
internal-reader handoff
       |
       v
JCL processing
       |
       v
job execution
       |
       v
SDSF visibility
```

Part 1 validates the ingress path and preserves downstream failures without misclassifying them.

Part 2 validates channel-dependent behavior: the controlled identity is denied through TSO/E `SUBMIT`, while a syntactically valid `IEFBR14` job is accepted through the FTP/JES level-1 path, assigned a JES job ID and completes with condition code `0000`.

This is **VALIDATED LOCALLY** as a trust-boundary experiment. It is not a claim that RACF owns FTP or JES2.

---

## 7. Communications Server integration

Communications Server owns:

```text
TCP/IP
FTP
TN3270
routing
listeners
Policy Agent / PAGENT
TTLSRule
AT-TLS
network transport
```

RACF owns the associated identity, SAF and cryptographic authorization questions.

### Cryptographic handoff

Current state:

```text
Communications Server
certificate / service inventory
        |
        v
RACF Lab 31
authorization baseline
        |
        v
RACF Lab 32
controlled RACDCERT delegation
        |
        v
RACF Lab 33
LAB33CERT + LAB33RING
        |
        v
Communications Server consumption
CROSS-DOMAIN / REQUIRES EVIDENCE
```

The retained RACF objects do not prove that Policy Agent, System SSL, AT-TLS or a TLS-protected network path is active.

### TN3270 authentication surface — Lab 35

Lab 35 compares controlled authentication outcomes and validates a **limited response distinction** at the TN3270/TSO logon surface.

The result must remain narrowly worded. It is evidence of observed response differentiation in the tested environment, not a universal claim about every z/OS installation.

### TN3270 transport — Lab 36

Lab 36 moves down the stack and validates cleartext TN3270 transport on the tested path through packet-level evidence.

This is **VALIDATED LOCALLY**.

The network engineering itself remains owned by Communications Server.

---

## 8. Lab 37 and the USS/network boundary

Lab 37 Part 1 validates readiness without fabricating completion.

Validated:

```text
OMVS shell
native TCP/IP visibility
C/C++ toolchain readiness
controlled USS workspace
NC110 source staging preparation
```

The external LCS/ETH1 path is isolated as the blocker.

The lab intentionally stops before NC110 execution.

State:

```text
Part 1 readiness             VALIDATED LOCALLY
LCS/ETH1 connectivity issue  BLOCKER DOCUMENTED
NC110 execution              PENDING
Part 2                       PENDING
```

This is a useful Architecture V2 example because a documented blocker is preserved as engineering evidence rather than rewritten as success.

---

## 9. Workload-automation integration

The scheduler repository owns scheduling logic and state.

A future security path is:

```text
scheduler identity
       |
       v
RACF / SAF
       |
       v
JES authority
       |
       v
scheduled workload
```

Security questions include the minimum authority required to submit, inspect and control designed workloads.

State: **CROSS-DOMAIN / REQUIRES EVIDENCE**.

No scheduler/RACF end-to-end least-privilege chain is claimed here.

---

## 10. CICS and Db2 integration

### CICS

Potential RACF-side concerns include:

- region identity;
- user authentication;
- transaction/resource authorization;
- dataset access;
- started-task security.

State: **PLANNED / REQUIRES EVIDENCE**.

CICS resource engineering remains in the CICS repository.

### Db2

Potential RACF-side concerns include:

- subsystem identities;
- started tasks;
- datasets;
- operational boundaries;
- external security integration.

State: **PLANNED / REQUIRES EVIDENCE**.

Db2 SQL authorization and subsystem engineering remain in the Db2 repository.

---

## 11. Core z/OS integration

The central z/OS engineering repository owns:

- IPL and initialization;
- PARMLIB;
- system-wide configuration;
- JES2 engineering;
- storage infrastructure;
- SMF infrastructure;
- Health Checker infrastructure;
- diagnostics and recovery.

RACF consumes that context when interpreting protected resources and authorization.

Examples already present in the security evidence include:

- Health Checker security findings;
- JES2/SDSF/OPERCMDS review;
- APF/PROGRAM/LINKLIST security analysis;
- zFS backing-dataset protection.

---

## 12. Effective-authority model

A recurring repository principle is:

```text
configured RACF state
        |
        +--> class active?
        +--> generic processing?
        +--> RACLIST?
        +--> matching profile?
        +--> UACC?
        +--> explicit user entry?
        +--> group authority?
        +--> cached state?
        |
        v
effective authority
        |
        v
functional result
```

This is why later labs prefer functional allow/deny evidence over administrative output alone.

---

## 13. RACLIST and runtime state

Labs 30-33 demonstrate why database state and effective runtime state must be distinguished for RACLISTed classes.

```text
RACF database change
       |
       v
RACLIST cache
       |
       v
SETROPTS RACLIST(class) REFRESH
       |
       v
effective runtime authorization
```

A rollback is not considered fully demonstrated until the effective behavior is restored.

---

## 14. Least-privilege model

The repository avoids proving a narrow function by granting broad global authority.

Preferred pattern:

```text
DENIED
   |
   v
minimum function-specific authorization
   |
   v
ALLOWED
   |
   v
remove authorization
   |
   v
refresh if required
   |
   v
DENIED AGAIN
```

Examples include UNIXPRIV and RACDCERT delegation.

Controlled identities are preferred over experimental changes to critical subsystem IDs.

---

## 15. Evidence and rollback

A change-oriented security lab should answer:

1. What was the baseline?
2. What operation was denied or exposed?
3. What minimum change was made?
4. Was a runtime refresh required?
5. What functional result changed?
6. What was rolled back?
7. Was rollback validated functionally?
8. What evidence was retained?

Depending on scope, evidence can include:

```text
README
commands
screenshots
findings
risk analysis
evidence manifest
before / after state
rollback procedure
rollback proof
limitations
publication review
```

Read-only labs do not require artificial rollback documentation.

---

## 16. Publication-security model

Security evidence receives strict publication review.

Do not publish unnecessary:

- passwords, credentials, tokens or secrets;
- private keys;
- private IP addresses;
- MAC addresses;
- host adapter names or identifiers;
- terminal/session identifiers;
- unrelated host infrastructure;
- sensitive information not required to establish the technical result.

Where possible, use synthetic identities, narrow resource names and sanitized evidence.

---

## 17. Architecture V2 lifecycle

The portfolio lifecycle is:

```text
Discover
  -> Baseline
  -> Configure
  -> Operate
  -> Observe
  -> Diagnose
  -> Recover
  -> Improve
  -> Automate
  -> Integrate
```

RACF labs do not need to force every stage into every exercise.

Typical security-change mapping:

```text
Discover / Baseline
        |
        v
Observe effective authority
        |
        v
Configure minimal change
        |
        v
Validate operation
        |
        v
Recover / rollback
        |
        v
Validate restored control
        |
        v
Document / integrate
```

---

## 18. Evidence maturity

The repository has evolved through several levels of engineering evidence:

```text
profile inspection
      |
      v
risk interpretation
      |
      v
controlled identity
      |
      v
functional allow / deny
      |
      v
least-privilege change
      |
      v
rollback proof
      |
      v
cross-domain trust-boundary analysis
```

This progression is more meaningful than assigning the same maturity label to every lab.

---

## 19. Production tracks

RACF/SAF contributes security controls to several portfolio production tracks.

### Secure Batch Application

```text
identity
 -> RACF/SAF
 -> datasets / JES controls
 -> batch workload
 -> audit evidence
```

### Secure Network Service

```text
service identity
 -> RACF/SAF
 -> certificate/key-ring authorization
 -> network service
 -> transport security
 -> observability
```

The RACF cryptographic side is partially validated; network consumption remains cross-domain work.

### Enterprise Batch Operations

```text
operator / scheduler identity
 -> RACF authority
 -> JES2 / SDSF
 -> workload control
```

Parts of the RACF operator-authority model are locally validated; scheduler-specific integration requires evidence.

### Problem Determination

Security incidents can require correlation among:

```text
identity
authorization
command result
subsystem evidence
SMF / audit evidence
network evidence
```

Cross-repository correlation remains an integration objective rather than a completed universal capability.

---

## 20. Current capability state

### Validated locally

Evidence exists for:

- RACF users, groups and privileged attributes;
- dataset profiles, UACC and PERMIT;
- WARNING and audit controls;
- controlled test identities;
- functional allow/deny tests;
- temporary and group-based authorization;
- cleanup and rollback;
- started-task and technical-user security review;
- OMVS / UID / GID security;
- zFS backing-dataset protection;
- SAF/FACILITY/OPERCMDS analysis;
- JES2/SDSF authority review from the security side;
- APF/PROGRAM/LINKLIST security review;
- effective-authority analysis;
- UNIXPRIV / UNIXMAP;
- controlled UNIXPRIV delegation;
- RACLIST behavior;
- RACF certificate/key-ring authorization;
- controlled RACDCERT delegation;
- synthetic certificate/key-ring lifecycle;
- FTP-to-JES trust-boundary behavior;
- TN3270/TSO authentication-response analysis;
- cleartext TN3270 transport on the tested path;
- Lab 37 Part 1 readiness and blocker isolation.

### Cross-domain / requires evidence

- `LAB33CERT` / `LAB33RING` consumption by Communications Server;
- AT-TLS validation using the retained RACF objects;
- scheduler service-ID / JES least privilege;
- SERVAUTH end-to-end integration;
- CICS external security integration;
- Db2 external RACF integration;
- cross-repository SMF security-event correlation;
- NC110 execution and Lab 37 Part 2.

---

## 21. Branch and change discipline

Repository-wide documentation changes should use dedicated documentation branches.

Security experiments should continue to use narrow lab branches where practical.

Before merging:

```text
baseline known
documentation/evidence reviewed
scope explicit
publication safety checked
rollback state documented where relevant
working tree clean
```

Architecture V2 documentation must not silently reclassify a planned capability as validated.

---

## 22. Learning journey versus runtime architecture

A learning sequence is not the same as a runtime dependency graph.

A learner may encounter:

```text
TSO / ISPF
 -> JCL
 -> scheduler
 -> REXX
 -> USS
 -> RACF / SAF
```

but runtime relationships are cross-cutting:

```text
                     RACF / SAF
                        |
       +----------------+----------------+
       |                |                |
      JES2             USS             TCP/IP
       |                |                |
    workload         process          service
```

Documentation should not imply that one technology is architecturally "after" another merely because it appears later in a learning path.

---

## 23. Navigation and handoff rules

Every cross-repository relationship should identify:

1. which repository owns the technology;
2. which repository owns the security interpretation;
3. what evidence already exists;
4. what remains unvalidated;
5. where the next engineering handoff belongs.

Examples:

```text
RACF certificate/key ring
   -> Communications Server consumes it

RACF UNIXPRIV
   -> USS exercises the protected function

RACF JES authority
   -> JES2 executes the workload

RACF scheduler identity
   -> scheduler owns workload-control semantics
```

This prevents duplicate labs and inflated capability claims.

---

## 24. Target integration paths

### Cryptographic network path

```text
RACF Labs 31-33
       |
       v
LAB33CERT + LAB33RING
       |
       v
Communications Server
Policy Agent / TTLSRule / AT-TLS
       |
       v
TLS validation
       |
       v
SMF / observability
```

Current state after the RACF object lifecycle: **CROSS-DOMAIN / REQUIRES EVIDENCE**.

### USS security path

```text
RACF identity
 -> OMVS attributes
 -> UNIXPRIV
 -> USS protected operation
 -> functional evidence
```

Individual RACF-side functions are validated. Broader cross-repository production-like chains should be evidenced separately.

### Scheduler security path

```text
scheduler service identity
 -> RACF
 -> JES authority
 -> workload
 -> operational evidence
```

Current state: **CROSS-DOMAIN / REQUIRES EVIDENCE**.

---

## 25. Engineering rule

The security repository should become more integrated without becoming less precise.

The governing rule is:

> **Do not infer end-to-end security from one layer of evidence.**

A network listener does not prove RACF authorization. A RACF permit does not prove application behavior. A certificate/key ring does not prove TLS. A JES handoff does not by itself prove successful execution.

The portfolio is strongest when each repository proves its own layer and cross-repository tracks connect those proofs explicitly.

---

## Final role statement

`mainframe-racf-security-evidence` provides the evidence-backed RACF/SAF security layer of the IBM z/OS engineering portfolio.

Its mature engineering pattern is:

```text
identity
  + protected resource
  + effective authority
  + functional validation
  + least privilege
  + audit evidence
  + rollback
  + trust-boundary analysis
```

while subsystem-specific implementation remains with the repositories that own those technologies.

Back to:

- [Repository README](../README.md)
- [Portfolio](https://github.com/P-dot/P-dot)
- [Architecture V2](https://github.com/P-dot/zos-adcd-hercules-engineering-lab/tree/main/docs/architecture/v2)
- [Engineering Control](https://github.com/P-dot/zos-adcd-hercules-engineering-lab/tree/main/docs/engineering-control)
