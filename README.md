# Mainframe RACF Security Evidence

Hands-on RACF and SAF security engineering laboratory for IBM z/OS.

This repository documents practical security work performed in a controlled z/OS ADCD / Hercules environment. It has evolved from introductory RACF profile inspection into a broader security-engineering track covering effective authorization, least privilege, dataset protection, z/OS UNIX security, operational command authority, audit readiness, controlled hardening, and rollback validation.

> **Scope:** educational and engineering laboratory. This repository is not a production penetration test, compliance certification, or production security baseline.

---

## Repository mission

The objective is not simply to execute RACF commands.

The repository demonstrates a repeatable security-engineering process:

```text
Inventory
   |
   v
Baseline
   |
   v
Analyze effective authority
   |
   v
Identify exposure
   |
   v
Design minimal change
   |
   v
Validate functionally
   |
   v
Rollback
   |
   v
Validate rollback
   |
   v
Document evidence
```

The central question is:

> **Who can do what, to which protected resource, under which effective authority, and how can that result be demonstrated safely?**

---

## Current status

The repository currently contains two complementary lab series:

- **H1–H13** — controlled RACF access-control and remediation exercises.
- **Main security series through Lab 32** — wider RACF/SAF, OMVS, JES2/SDSF, OPERCMDS, APF, audit, UNIXPRIV and cryptographic effective-authority analysis.

Major validated areas include:

- RACF users and groups
- privileged attributes
- DATASET profiles
- UACC and PERMIT
- WARNING mode
- group-based authorization
- controlled test identities
- access-denial and violation evidence
- temporary access and cleanup
- rollback validation
- RACF global options
- z/OS Health Checker security findings
- started-task and technical-user review
- OMVS / UID / GID security
- zFS backing-dataset protection
- SAF general-resource controls
- audit, logging and accountability
- JES2 / SDSF / OPERCMDS authority
- APF / PROGRAM / LINKLIST exposure
- FACILITY and OPERCMDS analysis
- effective-authority validation
- UNIXPRIV and UNIXMAP
- controlled least-privilege delegation
- RACF digital-certificate and key-ring authorization
- RACDCERT least-privilege delegation
- FACILITY RACLIST cache / effective-authority validation

---

# Lab navigation

## H-series — controlled RACF authorization lifecycle

| Lab | Topic | Focus |
|---|---|---|
| [H1](h1-racf-sandbox-safe-dataset-profiles/) | RACF Sandbox | Safe dataset profiles |
| [H2](h2-uacc-abuse-open-dataset-exposure/) | UACC Abuse | Open dataset exposure |
| [H3](h3-racf-permit-abuse-least-privilege-remediation/) | PERMIT Abuse | Least-privilege remediation |
| [H4](h4-racf-warning-mode-controlled-exposure/) | WARNING Mode | Controlled exposure |
| [H5](h5-racf-audit-controls-failure-evidence-readiness/) | AUDIT Controls | Failure evidence readiness |
| [H6](h6-racf-health-checker-active-class-baseline/) | Health Checker | UNIXPRIV / TEMPDSN / OPERCMDS baseline |
| [H7](h7-racf-test-identity-lab/) | Test Identity | Controlled non-production identity |
| [H8](h8-racf-real-access-test-lab/) | Real Access Test | Functional authorization validation |
| [H9](h9-racf-violation-evidence-lab/) | Violation Evidence | Denial and evidence collection |
| [H10](h10-racf-temporary-access-remediation-lab/) | Temporary Access | Grant/remediation lifecycle |
| [H11](h11-racf-group-based-access-lab/) | Group Access | Group-based authorization |
| [H12](h12-racf-access-cleanup-review-lab/) | Access Cleanup | Review and removal |
| [H13](h13-racf-restricted-user-impact-safe-rollback-lab/) | Restricted User | Impact analysis and safe rollback |

The H-series develops a progression from basic profile protection to functional proof:

```text
profile
 -> exposure
 -> authorization
 -> test identity
 -> allow / deny
 -> evidence
 -> remediation
 -> rollback
```

---

## Main RACF / SAF security series

| Lab | Topic | Focus |
|---|---|---|
| [Lab 02](lab-02-racf-global-options-review/) | RACF Global Options | Installation-level security configuration |
| [Lab 03](lab-03-racf-health-checker-sensitive-resources/) | Health Checker | `RACF_SENSITIVE_RESOURCES` |
| [Lab 04](lab-04-racf-security-model-baseline/) | Security Model | RACF baseline |
| [Lab 05](lab-05-started-task-security-review/) | Started Tasks | Security identity review |
| [Lab 06](lab-06-technical-users-privilege-review/) | Technical Users | Privilege analysis |
| [Lab 07](lab-07-zos-unix-omvs-security-baseline/) | z/OS UNIX / OMVS | UNIX identity/security baseline |
| [Lab 08](lab-08-racf-dataset-protection-baseline/) | Dataset Protection | Sensitive dataset coverage |
| [Labs 09–10](lab-09-y-10-zfs-saf-controls-review/) | zFS + SAF | Backing datasets and SAF controls |
| [Lab 11](lab-11-racf-audit-logging-accountability-baseline/) | Audit | Logging and accountability |
| [Lab 12](lab-12-jes2-sdsf-opercmds-console-authority-review/) | JES2 / SDSF / OPERCMDS | Operational authority |
| [Lab 13](lab-13-apf-program-authorized-libraries-review/) | APF / PROGRAM | Authorized-library exposure |
| [Lab 14](lab-14-final-racf-omvs-security-audit-report/) | Security Audit | Consolidated audit and hardening roadmap |
| [Lab 25](lab-25-facility-opercmds-security-review/) | FACILITY / OPERCMDS | General-resource review |
| [Lab 26](lab-26-racf-opercmds-effective-authority-analysis/) | OPERCMDS | Effective-authority analysis |
| [Lab 27](lab-27-racf-opercmds-controlled-hardening-validation/) | OPERCMDS Hardening | Controlled change and validation |
| [Lab 28](lab-28-racf-ispf-group-dataset-access-control/) | ISPF / Groups / DATASET | Functional group-based access |
| [Lab 29](lab-29-racf-unixpriv-security-baseline-effective-privilege-analysis/) | UNIXPRIV | Effective privileged-UNIX baseline |
| [Lab 30](lab-30-racf-unixpriv-controlled-delegation-validation/) | UNIXPRIV Delegation | Least privilege and rollback |
| [Lab 31](lab-31-racf-digital-certificate-trust-keyring-security-baseline/) | RACF Digital Certificates / Key Rings | Cryptographic authorization baseline |
| [Lab 32](lab-32-racf-controlled-cryptographic-delegation-keyring-validation/) | RACDCERT Delegation | Function-specific least privilege, RACLIST behavior and rollback |

> The numbering reflects the repository's actual historical development. Missing numbers are not silently represented as completed labs.

---

# Selected engineering milestones

## Effective authority, not just profile presence

A recurring principle throughout the repository is:

```text
profile exists
     !=
effective access
```

RACF authorization can depend on class state, profile matching, UACC, explicit user entries, group membership, RACLIST state and other installation context.

For that reason, later labs increasingly combine configuration inspection with functional validation.

---

## Controlled test identities

Security changes should not be tested casually against critical technical IDs.

The repository therefore developed controlled test identities so that authorization can be changed and validated without unnecessarily modifying important subsystem or service identities.

This enables the stronger pattern:

```text
administrative actor
       |
       v
controlled RACF change
       |
       v
non-privileged test subject
       |
       v
functional result
```

---

## Lab 14 — audit consolidation

Lab 14 consolidates the earlier RACF / OMVS audit work into a professional security-review structure.

The scope includes:

- privileged users and technical IDs;
- started-task identities;
- UID(0) exposure;
- sensitive DATASET profile coverage;
- zFS backing datasets;
- FACILITY;
- UNIXPRIV;
- SERVAUTH;
- OPERCMDS;
- JESSPOOL;
- JESJOBS;
- SDSF;
- audit/accountability posture;
- APF;
- PROGRAM;
- LINKLIST.

Its purpose is analysis and hardening planning rather than claiming production certification.

---

## Labs 25–27 — OPERCMDS progression

These labs form a deliberate progression:

```text
FACILITY / OPERCMDS inventory
          |
          v
effective-authority analysis
          |
          v
controlled hardening
          |
          v
validation
```

This moves the repository beyond read-only review while preserving controlled change and evidence.

---

## Labs 29–30 — UNIXPRIV least privilege

Lab 29 establishes the RACF/SAF baseline for privileged z/OS UNIX functions.

The validated environment includes an active, generic and RACLISTed `UNIXPRIV` class and protected `SUPERUSER.FILESYS*` resources.

Lab 30 then demonstrates a complete controlled authorization lifecycle:

```text
DENIED
   |
   v
minimal UNIXPRIV grant
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

The test subject remains globally non-privileged: the lab does not solve the problem by granting UID(0), `SPECIAL`, `OPERATIONS`, or broad superuser authority.

This is one of the repository's clearest demonstrations of least privilege plus functional rollback validation.

---

## Labs 31–32 — RACF cryptographic authorization and controlled delegation

Labs 31 and 32 extend the RACF/SAF security track into digital-certificate and key-ring authorization.

Lab 31 establishes the read-only cryptographic authorization baseline. It identifies the relevant `IRR.DIGTCERT.*` FACILITY controls that are present in the laboratory and validates the effective authorization boundary with the controlled `H7USER` identity.

Lab 32 then demonstrates a controlled RACDCERT delegation lifecycle using separate administrative controls for `LISTRING`, `ADDRING`, and `DELRING`.

```text
DENIED
   |
   v
minimum function-specific authority
   |
   v
ALLOWED
   |
   v
controlled key-ring creation
   |
   v
separate deletion boundary
   |
   v
controlled cleanup
   |
   v
authorization rollback
   |
   v
FACILITY RACLIST refresh
   |
   v
DENIED AGAIN
```

The lab also demonstrates that, for a RACLISTed class, changing the RACF database is not the same as changing effective runtime authority until the relevant RACLIST cache is refreshed.

No real service certificate, private key, or production-like network identity is created by Lab 32.

The next planned step is Lab 33, which will move from authorization mechanics into a controlled laboratory certificate and dedicated key-ring lifecycle before handoff to Communications Server for AT-TLS integration.

---

# RACF / SAF in the wider z/OS ecosystem

RACF is a cross-cutting security layer.

```text
                         RACF / SAF
                            |
        +-------------------+-------------------+
        |                   |                   |
        v                   v                   v
     JES2/SDSF           USS/OMVS             TCP/IP
        |                   |                   |
        v                   v                   v
    OPERCMDS             UNIXPRIV            SERVAUTH
        |
        +-------------------+-------------------+
                            |
                            v
                    protected workloads
```

This repository owns the **RACF/SAF security interpretation and validation**. It does not duplicate subsystem engineering from the repositories that own those technologies.

---

## Integration boundaries

### z/OS UNIX System Services

RACF-side topics include:

```text
OMVS segment
UID / GID
UNIXMAP
UNIXPRIV
effective UNIX authorization
```

General USS administration belongs to the dedicated USS repository.

### JES2 / SDSF

This repository covers security boundaries such as operational authority, general-resource protection and effective command access.

JES2 execution and spool engineering remain part of the central z/OS engineering track.

### Communications Server

Cross-repository security integration now also includes a defined certificate/key-ring boundary. The Communications Server repository owns network-service certificate inventory, PAGENT, TTLSRule, AT-TLS and transport validation. This repository owns RACF certificate/key-ring administrative authorization, `IRR.DIGTCERT.*`, effective authority and least-privilege RACDCERT delegation.

TCP/IP engineering itself remains in the Communications Server repository.

### Scheduler

The scheduler should eventually run with only the JES, dataset and operational authority required by its design.

That integration is planned; it is not presented here as already validated.

### Db2 and CICS

Future work may validate RACF-side subsystem boundaries, identities and external security controls.

Db2 SQL authorization and CICS resource engineering remain in their respective repositories.

---

# Ecosystem architecture

This repository is part of the broader **z/OS Engineering Laboratory**.

Master repository:

https://github.com/P-dot/zos-adcd-hercules-engineering-lab

Detailed RACF ecosystem role:

[`docs/ECOSYSTEM-INTEGRATION.md`](docs/ECOSYSTEM-INTEGRATION.md)

The wider engineering methodology is:

```text
Build -> Execute -> Observe -> Diagnose -> Correct -> Validate -> Document
```

For security-change labs, this repository extends it with:

```text
Baseline
 -> Deny
 -> Minimal change
 -> Allow
 -> Rollback
 -> Deny again
 -> Document
```

---

# Evidence philosophy

The repository preserves evidence because a successful command alone is not always sufficient proof.

Depending on the lab, evidence can include:

- command output;
- screenshots;
- findings;
- risk analysis;
- evidence manifests;
- before/after states;
- functional allow/deny tests;
- rollback procedures;
- rollback validation;
- troubleshooting notes;
- publication-security review.

A failed attempt may also be retained when it demonstrates a real platform limitation or troubleshooting path.

---

# Safety and publication rules

The repository is public, so evidence should be reviewed before publication.

Do not publish unnecessary:

- passwords or credentials;
- tokens or secrets;
- private keys;
- private IP addresses;
- MAC addresses;
- host adapter identifiers;
- terminal/session identifiers;
- host-side infrastructure details;
- sensitive information unrelated to the technical objective.

Security labs should use controlled identities and narrow changes wherever possible.

Critical technical identities should not be modified merely to demonstrate a concept.

---

# Lab environment

The repository is based on a controlled personal mainframe laboratory using:

- IBM z/OS ADCD;
- Hercules;
- 3270 / TSO/E;
- ISPF;
- SDSF;
- RACF;
- z/OS UNIX / OMVS;
- native RACF and SAF facilities available in the environment.

Some behavior is installation- and release-dependent. The repository documents the observed laboratory state rather than assuming that every production z/OS installation is configured identically.

---

# Engineering principles

1. **Read before changing.** Establish the baseline first.
2. **Analyze effective authority.** Do not stop at profile existence.
3. **Use least privilege.** Grant the smallest capability that satisfies the test.
4. **Prefer controlled identities.** Avoid unnecessary changes to critical service IDs.
5. **Test functionally.** A successful RACF command is not always proof of effective access.
6. **Plan rollback before change.** Recovery is part of the lab design.
7. **Validate rollback.** Restore the security behavior, not merely the profile text.
8. **Preserve evidence.** Make results reproducible and auditable.
9. **Separate lab findings from production claims.** ADCD/Hercules is a training environment.
10. **Keep subsystem ownership clear.** RACF secures other components; it does not replace them.

---

# Current maturity

The repository has progressed from:

```text
LISTUSER
LISTGRP
dataset profiles
```

to:

```text
UACC
PERMIT
WARNING
AUDIT
Health Checker
controlled identities
functional access tests
group authorization
rollback
```

and then into:

```text
started-task security
technical-user privilege
OMVS
zFS / SAF
JES2 / SDSF
OPERCMDS
APF / PROGRAM / LINKLIST
effective-authority analysis
UNIXPRIV
UNIXMAP
controlled least-privilege delegation
RACDCERT / IRR.DIGTCERT.* authorization
digital certificate / key-ring security baseline
function-specific cryptographic delegation
FACILITY RACLIST effective-authority validation
```

The current repository should therefore be understood as a **cross-domain z/OS security engineering track**, not only as an introductory RACF command collection.

---

# Planned integration work

Future work can extend the existing evidence into cross-repository chains such as:

```text
RACF -> Communications Server -> SMF
```

```text
USS -> RACF -> TCP/IP
```

```text
Scheduler -> RACF -> JES2
```

```text
RACF -> CICS / Db2 security boundary
```

```text
SAF authorization -> SMF evidence -> security analysis
```



A new cryptographic integration path is now partially validated:

```text
Communications Server certificate inventory
        -> RACF cryptographic authorization baseline
        -> RACF controlled delegation
        -> Lab 33 controlled certificate/key-ring lifecycle [planned]
        -> Communications Server AT-TLS / PAGENT / TTLSRule [planned]
```

These paths remain planned until implemented and validated with evidence.

---

# Repository role

`mainframe-racf-security-evidence` is the security-control and assurance component of the wider z/OS Engineering Laboratory.

Its role is to connect:

```text
identity
   +
RACF / SAF
   +
effective authorization
   +
functional validation
   +
audit evidence
   +
least privilege
   +
controlled hardening
   +
rollback
```

while leaving subsystem-specific engineering to the repositories that own those technologies.
