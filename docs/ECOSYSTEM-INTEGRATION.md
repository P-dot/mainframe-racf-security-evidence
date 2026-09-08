# RACF Security Evidence — Ecosystem Integration

## Purpose

This document defines the role of `mainframe-racf-security-evidence` within the wider z/OS Engineering Laboratory.

The repository is the cross-cutting security, authorization, least-privilege, audit-readiness and hardening layer of the ecosystem.

It does not replace the repositories that own JES2, USS, TCP/IP, Db2, CICS, storage, batch scheduling, or application development. Instead, it examines how RACF and SAF controls apply across those domains.

Master architecture:

https://github.com/P-dot/zos-adcd-hercules-engineering-lab

---

## 1. Architectural role

The repository sits across the ecosystem rather than beneath only one technology.

```text
                       RACF / SAF
                          |
      +-------------------+--------------------+
      |                   |                    |
      v                   v                    v
   JES2/SDSF            USS/OMVS             TCP/IP
      |                   |                    |
      v                   v                    v
 OPERCMDS              UNIXPRIV             SERVAUTH
      |
      +-------------------+--------------------+
      |                   |                    |
      v                   v                    v
   datasets             CICS                  Db2
      |
      v
  access control
```

The central function is:

```text
identity
   |
   v
authorization
   |
   v
effective access
   |
   v
evidence
   |
   v
risk analysis
   |
   v
controlled remediation
   |
   v
rollback
```

---

## 2. Repository responsibility

The repository owns practical security work around:

- RACF user profiles;
- RACF group profiles;
- privileged attributes;
- dataset profiles;
- UACC;
- PERMIT;
- WARNING mode;
- group-based access;
- test identities;
- access validation;
- violation evidence;
- temporary access;
- rollback;
- Health Checker security findings;
- RACF classes;
- SAF general resources;
- FACILITY;
- OPERCMDS;
- UNIXPRIV;
- UNIXMAP;
- OMVS identity exposure;
- UID/GID mapping;
- started-task identities;
- JES2/SDSF authority review;
- console-command authority;
- audit and accountability;
- SMF-related security evidence;
- APF-related exposure review;
- PROGRAM-class review;
- LINKLIST-related protection review;
- zFS backing-dataset security;
- effective-authority analysis;
- least-privilege delegation;
- controlled hardening;
- audit-style reporting;
- risk registers;
- production recommendations.

---

## 3. What this repository does not own

### USS administration

General shell, filesystem, process, path and POSIX administration belongs to:

```text
UNIX_System_Services-
```

RACF uses USS only where required to validate security behavior.

### Communications Server

TCP/IP configuration, TN3270, FTP, network services and Communications Server operations belong to:

```text
zos-communications-server-network-lab
```

RACF covers the authorization boundary where SAF classes or identities affect those services.

### JES2 and core system engineering

JES2 internals, spool, initialization, SMF collection, WLM, storage and system recovery belong to:

```text
zos-adcd-hercules-engineering-lab
```

This repository reviews security exposure around those components.

### Scheduler

Scheduling logic belongs to:

```text
zos-batch-scheduler
```

RACF can later validate the authority required by scheduler identities and operators.

### Db2

Db2 subsystem administration and SQL belong to:

```text
DB2-
```

RACF may cover subsystem access boundaries and external security controls, but not SQL education.

### CICS

CICS resource definition and transaction processing belong to:

```text
CICS
```

RACF can later validate external security integration, but does not own CICS itself.

---

## 4. Repository structure

The repository contains two historical lab series.

### H-series

```text
h1  through h13
```

These labs focus strongly on:

- safe dataset-profile exercises;
- UACC exposure;
- PERMIT misuse/remediation;
- WARNING mode;
- auditing;
- active-class baselines;
- controlled identities;
- real access tests;
- violation evidence;
- temporary access;
- group access;
- cleanup;
- rollback.

### Main lab series

The second series includes:

```text
lab-02
lab-03
lab-04
lab-05
lab-06
lab-07
lab-08
lab-09-y-10
lab-11
lab-12
lab-13
lab-14
lab-25
lab-26
lab-27
lab-28
lab-29
lab-30
```

This series expands into cross-domain z/OS security analysis.

---

## 5. H1 — Safe Dataset Profiles

The H-series begins with a safe RACF dataset-profile sandbox.

Architectural significance:

```text
dataset
   |
   v
RACF profile
   |
   v
access rule
   |
   v
controlled test
```

This creates the basic model used throughout later labs.

---

## 6. H2 — UACC Exposure

H2 examines open dataset exposure through UACC.

The security principle is:

```text
broad default access
        |
        v
larger exposure
```

The repo uses this to establish why explicit, narrow authorization is preferable.

---

## 7. H3 — PERMIT and Least Privilege

H3 moves from identifying broad access to remediating it.

The conceptual lifecycle is:

```text
existing permission
      |
      v
effective access
      |
      v
risk interpretation
      |
      v
least-privilege remediation
```

---

## 8. H4 — WARNING Mode

H4 introduces the distinction between enforcement and observation.

WARNING can help assess impact before strict enforcement.

The repository treats it as a controlled security state, not as equivalent to fully protected access.

---

## 9. H5 — Audit Controls

H5 strengthens the evidence model.

Security work is not complete when authorization exists.

The repository also asks:

```text
Was access attempted?
Was it allowed?
Was it denied?
Can the event be demonstrated?
```

---

## 10. H6 — Active-Class Baseline

H6 examines RACF class activation and Health Checker context.

The lab explicitly includes classes such as:

```text
UNIXPRIV
TEMPDSN
OPERCMDS
```

This is an important architectural bridge from simple profile administration into installation-wide security posture.

---

## 11. H7 — Controlled Test Identity

H7 introduces a dedicated test identity.

This is a major methodological improvement.

Instead of validating changes against critical system identities, the repo can use a controlled subject.

Pattern:

```text
admin identity
      |
      v
security change
      |
      v
controlled test identity
      |
      v
functional validation
```

---

## 12. H8 — Real Access Test

H8 goes beyond profile inspection.

The repository validates whether effective access matches RACF configuration.

That distinction is central:

```text
profile definition
      !=
functional proof
```

---

## 13. H9 — Violation Evidence

H9 explicitly captures denied access.

This creates evidence for:

- policy enforcement;
- troubleshooting;
- audit review;
- control validation.

---

## 14. H10 — Temporary Access

H10 examines temporary authorization and remediation.

The important lifecycle is:

```text
need
 |
 v
temporary access
 |
 v
validation
 |
 v
remove access
 |
 v
verify final state
```

---

## 15. H11 — Group-Based Access

H11 moves authorization away from one-off user permissions toward group-based models.

Conceptually:

```text
user
 |
 v
group
 |
 v
resource authorization
```

This is more scalable and closer to enterprise access-control practice.

---

## 16. H12 — Access Cleanup

H12 reviews stale or excessive access.

Security is treated as a lifecycle:

```text
grant
 |
 v
use
 |
 v
review
 |
 v
cleanup
```

not as a one-time configuration task.

---

## 17. H13 — Restricted User and Rollback

H13 emphasizes impact analysis and safe rollback.

The repository therefore treats security changes as operational changes that require recovery planning.

---

## 18. Lab 02 — RACF Global Options Review

The main sequence expands from individual profiles into installation-level RACF configuration.

The architectural question becomes:

```text
How is RACF configured globally?
```

This is necessary before interpreting individual access decisions.

---

## 19. Lab 03 — Health Checker Sensitive Resources

Lab 03 connects RACF analysis with z/OS Health Checker.

This adds an independent system-health perspective to manual RACF inspection.

---

## 20. Lab 04 — Security Model Baseline

Lab 04 establishes a wider security baseline.

A baseline enables later comparisons:

```text
BEFORE
  |
  v
change
  |
  v
AFTER
```

---

## 21. Lab 05 — Started Task Security

Lab 05 connects started tasks to security identities.

This is a key cross-domain relationship:

```text
started task
      |
      v
RACF identity
      |
      v
effective authority
```

This relationship matters to Communications Server, JES2, Db2, CICS and other subsystem workloads.

---

## 22. Lab 06 — Technical Users

Lab 06 reviews technical identities and privilege.

This separates:

```text
human administrator
```

from:

```text
service / technical identity
```

The distinction matters because production systems should avoid unnecessary broad privilege in long-running service identities.

---

## 23. Lab 07 — z/OS UNIX / OMVS Baseline

Lab 07 introduces the RACF side of z/OS UNIX identity and privilege.

The integration path is:

```text
RACF user
   |
   v
OMVS segment
   |
   v
UID / GID
   |
   v
USS effective identity
```

General USS administration remains outside this repository.

---

## 24. Lab 08 — Dataset Protection Baseline

Lab 08 returns to dataset security at wider scope.

It examines whether sensitive data has visible RACF profile protection.

This supports later work around system datasets, application datasets and zFS backing datasets.

---

## 25. Labs 09–10 — zFS and SAF

These labs bridge:

```text
zFS
 |
 v
backing dataset
 |
 v
RACF DATASET control
```

and:

```text
z/OS service
 |
 v
SAF class
 |
 v
general-resource authorization
```

This is an important point: USS filesystem security and MVS dataset security can intersect.

---

## 26. Lab 11 — Audit, Logging and Accountability

Lab 11 examines whether security-relevant activity is observable and attributable.

The broader chain is:

```text
identity
 |
 v
action
 |
 v
authorization decision
 |
 v
security event
 |
 v
audit evidence
```

---

## 27. Lab 12 — JES2 / SDSF / OPERCMDS / Console Authority

Lab 12 is one of the clearest cross-domain security labs.

It connects RACF to operational control:

```text
operator identity
      |
      v
RACF / SAF
      |
      +--> OPERCMDS
      +--> SDSF
      +--> JES resources
      |
      v
effective operational authority
```

JES2 and SDSF remain operational components; RACF owns the authorization interpretation in this repo.

---

## 28. Lab 13 — APF / PROGRAM / Authorized Libraries

Lab 13 moves into highly sensitive system-program exposure.

It reviews relationships among:

- APF;
- PROGRAM;
- LINKLIST;
- dataset protection;
- authorized libraries.

This is a review and hardening perspective.

It does not mean the repository implements authorized Assembler programs.

---

## 29. Lab 14 — Final RACF / OMVS Audit

Lab 14 consolidates the earlier audit path.

It covers findings around:

- privileged RACF users;
- technical IDs;
- started tasks;
- OMVS;
- UID(0);
- DATASET protection;
- zFS backing datasets;
- FACILITY;
- UNIXPRIV;
- SERVAUTH;
- OPERCMDS;
- JESSPOOL;
- JESJOBS;
- SDSF;
- logging/accountability;
- APF;
- PROGRAM;
- LINKLIST.

The result is an audit-style hardening roadmap rather than a single configuration change.

---

## 30. Lab 25 — FACILITY / OPERCMDS Review

Lab 25 begins a later hardening sequence around general-resource authorization.

Architecturally, this is the transition from:

```text
inventory
```

to:

```text
effective authority analysis
```

and eventually:

```text
controlled remediation
```

---

## 31. Lab 26 — OPERCMDS Effective Authority

Lab 26 focuses on effective authority rather than profile presence alone.

This matters because a RACF profile can exist while actual access depends on:

- UACC;
- user entries;
- group entries;
- generic matching;
- class state;
- RACLIST state.

The key principle is:

```text
configured profile
      !=
effective authority
```

---

## 32. Lab 27 — OPERCMDS Controlled Hardening

Lab 27 applies controlled hardening and validates the result.

The pattern is:

```text
baseline
   |
   v
analyze effective authority
   |
   v
controlled change
   |
   v
refresh
   |
   v
functional validation
```

---

## 33. Lab 28 — ISPF Group and Dataset Access

Lab 28 reconnects group membership and dataset profiles with a practical ISPF workflow.

This demonstrates that RACF security is visible through normal user activity, not only through administrative commands.

---

## 34. Lab 29 — UNIXPRIV Baseline

Lab 29 establishes a read-only baseline for privileged z/OS UNIX authorization.

It explicitly separates RACF/SAF security from general USS administration.

The validated state includes:

```text
UNIXPRIV
  ACTIVE
  GENERIC
  RACLISTed
```

Observed profiles include:

```text
SUPERUSER.FILESYS
SUPERUSER.FILESYS.CHANGEPERMS
SUPERUSER.FILESYS.CHOWN
SUPERUSER.FILESYS.MOUNT
```

The observed profiles use:

```text
UACC(NONE)
```

and the lab treats UID(0) mappings as an exposure inventory requiring careful dependency analysis.

---

## 35. Lab 29 — UNIXMAP

The lab demonstrates use of UNIXMAP as a compatible way to inspect UID mapping where another native search path was unavailable due to installation configuration.

This is valuable because the methodology does not change global RACF configuration merely to make an inquiry command work.

Pattern:

```text
preferred inquiry unavailable
        |
        v
document limitation
        |
        v
use safe compatible alternative
```

---

## 36. Lab 30 — Controlled UNIXPRIV Delegation

Lab 30 is one of the strongest functional validation labs in the repository.

It demonstrates:

```text
DENIED
   |
   v
minimal RACF grant
   |
   v
ALLOWED
   |
   v
rollback
   |
   v
DENIED
```

The same user, file and operation are used across the lifecycle.

---

## 37. Lab 30 — Least Privilege

The test subject remains globally non-privileged.

The lab explicitly avoids granting:

- UID(0);
- SPECIAL;
- OPERATIONS;
- broad UNIXPRIV wildcard authority;
- global superuser authority.

Instead, one narrowly scoped UNIXPRIV capability is delegated.

This is a direct least-privilege demonstration.

---

## 38. Lab 30 — RACLIST Refresh

The lab also demonstrates the operational importance of refreshing a RACLISTed class after authorization changes.

Conceptually:

```text
RACF database change
        |
        v
RACLIST cache
        |
        v
REFRESH
        |
        v
effective runtime authorization
```

---

## 39. Lab 30 — Rollback Proof

Rollback is not treated as complete merely because a `PERMIT ... DELETE` command succeeds.

The final denial proves the authorization was actually removed.

This is stronger evidence:

```text
configuration rollback
        +
functional rollback validation
```

---

## 40. Core security model

Across the repository, the recurring model is:

```text
WHO
 |
 v
identity

CAN DO WHAT
 |
 v
authorization

TO WHICH RESOURCE
 |
 v
profile / class

UNDER WHICH DEFAULT
 |
 v
UACC

WITH WHICH EFFECTIVE RESULT
 |
 v
ALLOW / DENY

WITH WHICH EVIDENCE
 |
 v
audit / screenshots / findings

AND HOW TO RECOVER
 |
 v
rollback
```

---

## 41. RACF and SAF

RACF is treated as the security manager.

SAF is the system interface through which many z/OS components request authorization.

Therefore the ecosystem view is:

```text
application / subsystem
        |
        v
       SAF
        |
        v
      RACF
        |
        v
authorization decision
```

This is why the repository naturally intersects many other repos.

---

## 42. Relationship with USS

The security integration path is:

```text
RACF
 |
 +--> OMVS segment
 |
 +--> UID / GID
 |
 +--> UNIXMAP
 |
 +--> UNIXPRIV
 |
 v
USS effective security
```

General shell and filesystem administration remain owned by the USS repository.

---

## 43. Relationship with Communications Server

Communications Server commonly depends on technical identities and SAF-controlled resources.

The intended architectural relationship is:

```text
Communications Server
        |
        v
technical identity
        |
        v
SAF request
        |
        v
RACF authorization
```

Relevant RACF-side topics can include:

- service identities;
- started-task mapping;
- SERVAUTH;
- dataset access;
- OMVS attributes;
- privileged UNIX capabilities.

Specific networking behavior remains in the Communications Server repo.

---

## 44. Relationship with JES2

JES2 owns batch execution.

RACF owns the security view of:

- operator authority;
- JES resource classes;
- command authority;
- SDSF-related control;
- started-task identity;
- dataset access.

Conceptually:

```text
operator / scheduler / user
        |
        v
      RACF
        |
        v
 JES2 / SDSF operation
```

---

## 45. Relationship with the Scheduler

The scheduler must eventually run under an identity with precisely enough authority to:

- submit work;
- inspect work;
- hold/release where designed;
- interact with JES2;
- access required datasets.

The intended integration is:

```text
scheduler identity
      |
      v
RACF
      |
      v
JES2 authority
      |
      v
scheduled workload
```

This is planned cross-repo integration, not yet a completed scheduler/RACF end-to-end lab.

---

## 46. Relationship with Db2

Db2 has its own authorization model, but z/OS and RACF still matter around:

- subsystem access;
- technical identities;
- started tasks;
- datasets;
- external security boundaries;
- operational commands.

The RACF repo should cover only the RACF/SAF side of such future integration.

---

## 47. Relationship with CICS

CICS also intersects RACF through:

- region identities;
- user authentication;
- transaction/resource authorization;
- datasets;
- started-task security.

The actual CICS resource model remains in the CICS repository.

---

## 48. Relationship with Storage

Sensitive datasets include:

- system libraries;
- application data;
- zFS backing datasets;
- logs;
- security-relevant data.

RACF's role is:

```text
dataset
 |
 v
profile
 |
 v
effective authority
```

Storage management itself remains in the central z/OS engineering repository.

---

## 49. Relationship with SMF

Security evidence may depend on SMF records and audit configuration.

The relationship is:

```text
security event
      |
      v
SMF / audit trail
      |
      v
evidence
```

SMF infrastructure and record-management engineering belong to the central repo.

---

## 50. Relationship with Health Checker

Health Checker provides an additional system-level assessment source.

The RACF repository uses it for security posture validation.

It does not own Health Checker itself.

---

## 51. Relationship with z_Assembly

Assembler can eventually interact with system services that trigger SAF checks.

However, no current repository evidence should be interpreted as implementing:

- RACF exits;
- SAF exits;
- authorized security modules;
- RACF control-block manipulation.

Those remain advanced future topics.

---

## 52. Validated capabilities

The current repository validates practical work in:

| Capability | State |
|---|---|
| RACF user review | Validated |
| RACF group review | Validated |
| privileged attribute analysis | Validated |
| dataset profile review | Validated |
| UACC exposure analysis | Validated |
| PERMIT analysis | Validated |
| least-privilege remediation | Validated |
| WARNING mode analysis | Validated |
| audit-control review | Validated |
| Health Checker RACF review | Validated |
| controlled test identity | Validated |
| functional access tests | Validated |
| violation evidence | Validated |
| temporary access lifecycle | Validated |
| group-based access | Validated |
| access cleanup | Validated |
| rollback validation | Validated |
| started-task security review | Validated |
| technical user review | Validated |
| OMVS security baseline | Validated |
| zFS backing-dataset review | Validated |
| SAF class review | Validated |
| RACF audit/accountability baseline | Validated |
| JES2/SDSF/OPERCMDS authority review | Validated |
| APF/PROGRAM/LINKLIST security review | Validated |
| FACILITY review | Validated |
| OPERCMDS effective-authority analysis | Validated |
| controlled OPERCMDS hardening | Validated |
| ISPF/group/dataset functional access | Validated |
| UNIXPRIV baseline | Validated |
| UNIXMAP UID(0) exposure review | Validated |
| controlled UNIXPRIV delegation | Validated |
| functional rollback proof | Validated |

---

## 53. Planned cross-repo capabilities

These should remain explicitly marked as planned:

| Capability | State |
|---|---|
| Scheduler service-ID security integration | Planned |
| Scheduler/JES2 least-privilege authority | Planned |
| Communications Server SERVAUTH end-to-end integration | Planned |
| CICS external security integration | Planned |
| Db2 external RACF integration | Planned |
| Assembler SAF-call integration | Planned |
| Production-style identity lifecycle automation | Planned |
| Cross-repo SMF security-event correlation | Planned |
| End-to-end enterprise security control chain | Planned |

---

## 54. Security methodology

The mature methodology used by the repo is:

```text
Inventory
   |
   v
Baseline
   |
   v
Interpret effective authority
   |
   v
Identify risk
   |
   v
Design minimal change
   |
   v
Apply
   |
   v
Refresh if required
   |
   v
Functional validation
   |
   v
Rollback
   |
   v
Functional rollback validation
   |
   v
Document
```

---

## 55. Read-only before change

A strong rule for future labs should be:

> Prefer inquiry and baseline collection before modifying RACF configuration.

This avoids making changes without understanding:

- profile matching;
- class state;
- group relationships;
- current ACLs;
- technical dependencies.

---

## 56. Effective access over profile presence

Future labs should continue emphasizing:

```text
"profile exists"
```

is not enough.

The important question is:

```text
What access does the identity effectively have?
```

---

## 57. Functional validation over command success

Similarly:

```text
PERMIT returned successfully
```

does not by itself prove the target operation is allowed.

The strongest evidence is a functional test.

---

## 58. Rollback as a first-class requirement

Any change-oriented security lab should contain:

- exact rollback command;
- expected final configuration;
- functional rollback test;
- evidence of restored control.

---

## 59. Critical identity protection

Future cross-domain labs must avoid experimental privilege removal from critical technical identities unless dependencies are fully understood.

Examples of sensitive service identities can include networking, subsystem or initialization tasks.

Controlled test identities should remain the preferred validation model.

---

## 60. Publication discipline

Public evidence must be screened for:

- passwords;
- tokens;
- secrets;
- private keys;
- IP addresses;
- MAC addresses;
- host-only network details;
- terminal/session identifiers;
- unnecessary user identifiers;
- host adapter names;
- infrastructure details unrelated to the lab.

Security repositories deserve especially strict review because evidence often contains privileged system information.

---

## 61. Evidence model

A strong RACF lab should include:

```text
README
commands
findings
risk analysis
evidence manifest
screenshots
rollback
security review
```

where applicable.

Not every read-only lab requires a rollback document.

---

## 62. Audit-style documentation

This repository is stronger when it separates:

```text
observation
finding
risk
recommendation
evidence
```

rather than blending them together.

---

## 63. Production-context language

The repository should continue clearly distinguishing:

```text
ADCD / Hercules lab observation
```

from:

```text
production recommendation
```

A lab weakness is not automatically proof that a production installation is insecure.

---

## 64. Controlled environment

The work should continue to be framed as:

- educational;
- reproducible;
- controlled;
- non-production;
- evidence-driven.

The value is in the methodology and demonstrated technical skill.

---

## 65. Future integration — RACF + Communications Server

Recommended future branch:

```text
integration/racf-network-smf
```

Possible flow:

```text
TCP/IP service
   |
   v
started-task identity
   |
   v
SAF resource
   |
   v
RACF decision
   |
   v
SMF evidence
```

Only implement this when each dependency is validated.

---

## 66. Future integration — USS + RACF + TCP/IP

Recommended cross-repo path:

```text
USS
 |
 v
RACF OMVS identity
 |
 v
UNIXPRIV / filesystem capability
 |
 v
TCP/IP service
```

This should avoid duplicating general USS or Communications Server teaching.

---

## 67. Future integration — Scheduler + RACF + JES2

Recommended path:

```text
scheduler service identity
        |
        v
RACF authorization
        |
        v
JES2 submission / control
        |
        v
scheduled workload
```

Security objective:

> Grant only the JES and dataset authority required by the scheduler design.

---

## 68. Future integration — CICS

Potential future path:

```text
user
 |
 v
RACF authentication / authorization
 |
 v
CICS region
 |
 v
transaction
```

This remains planned.

---

## 69. Future integration — Db2

Potential future path:

```text
technical identity
      |
      v
RACF / z/OS boundary
      |
      v
Db2 subsystem
```

Keep SQL privilege concepts in the Db2 repository.

---

## 70. Future integration — Security observability

A mature cross-repo security flow can become:

```text
access attempt
      |
      v
SAF / RACF decision
      |
      v
SMF evidence
      |
      v
analysis
      |
      v
finding
      |
      v
remediation
```

---

## 71. Recommended branch model

Security integration should continue using short-lived branches.

Examples:

```text
docs/racf-ecosystem-integration-v1
integration/racf-network-smf
integration/racf-scheduler-jes2
integration/racf-uss-unixpriv
integration/racf-cics-security
integration/racf-db2-security
fix/racf-documentation
```

Lifecycle:

```text
main
 |
 +--> branch
        |
        +--> change
        +--> evidence
        +--> security review
        +--> validation
        +--> rollback where applicable
        |
        v
       PR
        |
        v
      main
        |
        v
 delete branch
```

---

## 72. Root README status

The current root README represents an early stage of the repository.

It still describes introductory `LISTUSER` and `LISTGRP` work and references early planned labs.

The repository now contains substantially more mature work through Lab 30.

Therefore the root README should be rebuilt in a separate documentation branch after this integration document is merged.

Recommended branch:

```text
docs/racf-root-readme-v2
```

Do not mix that rebuild into the current ecosystem-integration branch.

---

## 73. Recommended root README future structure

The future root README should include:

- repository mission;
- current lab status;
- H-series explanation;
- main-series explanation;
- current Labs 25–30;
- major security domains;
- validated capabilities;
- architecture integration;
- selected evidence highlights;
- safety and publication scope;
- navigation table;
- learning path;
- production-vs-lab disclaimer.

---

## 74. Repository maturity

The repository has evolved from:

```text
LISTUSER / LISTGRP
```

through:

```text
dataset access
UACC
PERMIT
WARNING
audit
Health Checker
test identities
functional denial
group access
rollback
```

into:

```text
OMVS
UNIXPRIV
OPERCMDS
JES2/SDSF
APF
PROGRAM
LINKLIST
audit roadmap
effective-authority analysis
controlled hardening
least-privilege delegation
```

This is no longer an introductory RACF-only repository.

It is a cross-domain z/OS security engineering track.

---

## 75. Ecosystem security principle

The central architectural principle is:

> RACF should not duplicate subsystem engineering; it should prove who can do what, to which protected resource, under which effective authority, with evidence and rollback.

This keeps the repo focused while still making it central to the entire z/OS laboratory.

---

## 76. Target end state

The long-term architecture should support:

```text
                 RACF / SAF
                    |
   +----------------+----------------+
   |                |                |
   v                v                v
Scheduler         USS             TCP/IP
   |                |                |
   v                v                v
 JES2           UNIXPRIV         SERVAUTH
   |
   +----------------+----------------+
                    |
                    v
            enterprise workload
                    |
                    v
              SMF evidence
                    |
                    v
              audit analysis
```

RACF becomes the cross-cutting control plane for identity and authorization across the broader lab ecosystem.

---

## 77. Engineering rule

The repository should continue to follow this rule:

> Never treat a RACF command, profile, or ACL as sufficient evidence by itself when effective access can be safely tested.

The strongest lab pattern is:

```text
baseline
 -> denied
 -> minimal change
 -> allowed
 -> rollback
 -> denied
```

Lab 30 demonstrates exactly this model and provides a strong template for future controlled security integrations.

---

## 78. Final role statement

`mainframe-racf-security-evidence` is the z/OS Engineering Laboratory's security-control and assurance repository.

Its purpose is to connect:

- identity;
- RACF;
- SAF;
- effective authorization;
- subsystem access;
- audit evidence;
- least privilege;
- controlled hardening;
- rollback.

The repository should remain the source of truth for RACF-side security analysis across the wider ecosystem while leaving subsystem-specific engineering to the repositories that own those technologies.
