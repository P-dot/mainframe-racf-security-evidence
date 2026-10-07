# Mainframe RACF / SAF Security Engineering Evidence

Hands-on IBM z/OS security engineering focused on **RACF, SAF, effective authorization, least privilege, functional validation, audit evidence and controlled rollback**.

This repository is the security-control and assurance layer of the wider IBM z/OS engineering portfolio. It documents work performed in a controlled ADCD / Hercules environment and separates laboratory evidence from production claims.

> **Scope:** educational and engineering laboratory. This repository is not a production penetration test, compliance certification, or production security baseline.

## Quick navigation

| Destination | Purpose |
|---|---|
| [Portfolio](https://github.com/P-dot/P-dot) | Main z/OS engineering portfolio |
| [Ecosystem integration](docs/ECOSYSTEM-INTEGRATION.md) | RACF/SAF ownership and cross-repository boundaries |
| [Recruiter summary](docs/recruiter-summary.md) | Concise professional capability summary |
| [Architecture V2](https://github.com/P-dot/zos-adcd-hercules-engineering-lab/tree/main/docs/architecture/v2) | Portfolio architecture and lifecycle model |
| [Engineering Control](https://github.com/P-dot/zos-adcd-hercules-engineering-lab/tree/main/docs/engineering-control) | Baseline, change, recovery and maturity controls |
| [Core z/OS engineering](https://github.com/P-dot/zos-adcd-hercules-engineering-lab) | System-level engineering |
| [Communications Server](https://github.com/P-dot/zos-communications-server-network-lab) | TCP/IP, TN3270 and network engineering |
| [USS](https://github.com/P-dot/UNIX_System_Services-) | OMVS / POSIX runtime engineering |
| [Workload automation](https://github.com/P-dot/zos-batch-scheduler) | Scheduler engineering |

---

## Repository role

The central engineering question is:

> **Who can do what, to which protected resource, under which effective authority, and how can the result be demonstrated safely?**

The repository owns the RACF/SAF security interpretation of identities, protected resources and authorization decisions. It does **not** claim ownership of the subsystems that consume those controls.

```text
                         RACF / SAF
                            |
       +--------------------+--------------------+
       |                    |                    |
       v                    v                    v
 Identity & Access    Privilege Control     Trust Boundaries
       |                    |                    |
       v                    v                    v
 H1-H13 / Lab 28       Labs 25-33           Labs 34-37
       \                    |                    /
        +-------------------+-------------------+
                            |
                            v
                  Evidence & Assurance
              allow / deny / rollback
```

---

## Security capability map

### 1. Access-control lifecycle — H1-H13

The H-series develops the controlled authorization lifecycle from dataset-profile protection through functional access testing and rollback.

| Track | Evidence |
|---|---|
| Dataset protection | Safe profiles, UACC exposure, PERMIT analysis |
| Enforcement | WARNING mode and audit controls |
| Identity | Controlled non-production test identity |
| Functional proof | Allow/deny tests and violation evidence |
| Remediation | Temporary access, group authorization and cleanup |
| Recovery | Restricted-user impact analysis and rollback |

[Open the H-series](#h-series--controlled-racf-authorization-lifecycle)

### 2. RACF / SAF security baseline — Labs 02-14

This track expands from individual profiles into installation-level and cross-domain security review:

- RACF global options and Health Checker findings;
- started-task and technical-user identities;
- OMVS / UID / GID security;
- dataset and zFS backing-dataset protection;
- SAF general-resource controls;
- audit and accountability;
- JES2 / SDSF / OPERCMDS authority;
- APF / PROGRAM / LINKLIST exposure;
- consolidated RACF / OMVS security audit.

### 3. Effective authority and least privilege — Labs 25-30

```text
inventory
   -> effective-authority analysis
   -> controlled change
   -> refresh where required
   -> functional validation
   -> rollback
   -> functional rollback validation
```

This track includes FACILITY / OPERCMDS, practical group/dataset access, UNIXPRIV, UNIXMAP and narrow privilege delegation.

### 4. Cryptographic trust — Labs 31-33

The cryptographic-security track validates the RACF side of certificate and key-ring administration.

```text
Lab 31
authorization baseline
     |
     v
Lab 32
function-specific RACDCERT delegation
     |
     v
Lab 33
LAB33CERT + LAB33RING lifecycle
```

Lab 33 deliberately retains the synthetic `LAB33CERT`, its RACF-managed private key, `LAB33RING`, and their association after temporary administrative authority is removed. Their future consumption by Communications Server remains a separate cross-repository integration step.

### 5. Cross-domain trust boundaries — Labs 34-37

The latest labs apply the security model at subsystem boundaries without claiming ownership of those subsystems.

| Lab | Security question | Evidence state |
|---|---|---|
| [34](lab-34-ftp-jes-trust-boundary/) | FTP-to-JES ingress and channel-dependent authorization | **VALIDATED LOCALLY — Parts 1 and 2** |
| [35](lab-35-tn3270-tso-authentication-exposure/) | TN3270 / TSO authentication-response distinction | **VALIDATED LOCALLY** |
| [36](lab-36-tn3270-cleartext-transport-exposure/) | Cleartext TN3270 transport on the tested path | **VALIDATED LOCALLY** |
| [37 Part 1](lab-37-omvs-netcat-exposure-part-1/) | OMVS/NC110 readiness and LCS/ETH1 connectivity blocker | **PART 1 VALIDATED / BLOCKER DOCUMENTED** |

Lab 37 intentionally stops before NC110 execution. Part 2 remains pending.

---

## Validation vocabulary

This repository uses explicit evidence states.

| State | Meaning |
|---|---|
| **VALIDATED LOCALLY** | Demonstrated by evidence in this repository |
| **VALIDATED IN TARGET REPOSITORY** | Demonstrated by evidence owned by another repository |
| **CROSS-DOMAIN / REQUIRES EVIDENCE** | Architectural relationship identified, but the end-to-end path is not yet demonstrated |
| **PLANNED** | Future work; not presented as completed |
| **BLOCKER DOCUMENTED** | Execution could not continue, but the failure boundary was isolated and preserved as evidence |

A configuration object, successful administrative command or planned architecture is not automatically functional proof.

---

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

---

## Main RACF / SAF security series

| Lab | Topic | Focus |
|---|---|---|
| [02](lab-02-racf-global-options-review/) | RACF Global Options | Installation-level configuration |
| [03](lab-03-racf-health-checker-sensitive-resources/) | Health Checker | Sensitive-resource findings |
| [04](lab-04-racf-security-model-baseline/) | Security Model | RACF baseline |
| [05](lab-05-started-task-security-review/) | Started Tasks | Security identity review |
| [06](lab-06-technical-users-privilege-review/) | Technical Users | Privilege analysis |
| [07](lab-07-zos-unix-omvs-security-baseline/) | z/OS UNIX / OMVS | UNIX identity/security baseline |
| [08](lab-08-racf-dataset-protection-baseline/) | Dataset Protection | Sensitive dataset coverage |
| [09-10](lab-09-y-10-zfs-saf-controls-review/) | zFS + SAF | Backing datasets and SAF controls |
| [11](lab-11-racf-audit-logging-accountability-baseline/) | Audit | Logging and accountability |
| [12](lab-12-jes2-sdsf-opercmds-console-authority-review/) | JES2 / SDSF / OPERCMDS | Operational authority |
| [13](lab-13-apf-program-authorized-libraries-review/) | APF / PROGRAM | Authorized-library exposure |
| [14](lab-14-final-racf-omvs-security-audit-report/) | Security Audit | Consolidated hardening roadmap |
| [25](lab-25-facility-opercmds-security-review/) | FACILITY / OPERCMDS | General-resource review |
| [26](lab-26-racf-opercmds-effective-authority-analysis/) | OPERCMDS | Effective-authority analysis |
| [27](lab-27-racf-opercmds-controlled-hardening-validation/) | OPERCMDS Hardening | Controlled change and validation |
| [28](lab-28-racf-ispf-group-dataset-access-control/) | ISPF / Groups / DATASET | Functional group-based access |
| [29](lab-29-racf-unixpriv-security-baseline-effective-privilege-analysis/) | UNIXPRIV / UNIXMAP | Privileged-UNIX baseline |
| [30](lab-30-racf-unixpriv-controlled-delegation-validation/) | UNIXPRIV Delegation | Least privilege and rollback |
| [31](lab-31-racf-digital-certificate-trust-keyring-security-baseline/) | Certificates / Key Rings | Cryptographic authorization baseline |
| [32](lab-32-racf-controlled-cryptographic-delegation-keyring-validation/) | RACDCERT Delegation | Function-specific delegation and RACLIST behavior |
| [33](lab-33-racf-controlled-certificate-keyring-lifecycle/) | Certificate / Key Ring Lifecycle | Synthetic certificate, ring and CONNECT validation |
| [34](lab-34-ftp-jes-trust-boundary/) | FTP-to-JES | Trust-boundary and channel-dependent authorization |
| [35](lab-35-tn3270-tso-authentication-exposure/) | TN3270 / TSO | Authentication-response analysis |
| [36](lab-36-tn3270-cleartext-transport-exposure/) | TN3270 Transport | Cleartext exposure validation |
| [37 Part 1](lab-37-omvs-netcat-exposure-part-1/) | OMVS / NC110 | Readiness and connectivity-blocker isolation |

> The numbering reflects the repository's actual historical development. Missing numbers are not represented as completed labs.

---

## Selected engineering milestones

### Effective authority over profile presence

```text
profile exists
     !=
effective access
```

Later labs combine RACF configuration inspection with functional testing, class-state awareness and RACLIST behavior.

### Controlled identities

The repository uses controlled identities rather than casually modifying critical technical IDs.

```text
administrative actor
       -> controlled RACF change
       -> controlled test subject
       -> functional result
```

### Least privilege plus rollback

Labs 27, 30 and 32 demonstrate increasingly mature controlled-change patterns. Lab 30 provides a particularly clear lifecycle:

```text
DENIED
   -> minimum UNIXPRIV grant
   -> ALLOWED
   -> rollback
   -> DENIED
```

### Cryptographic authorization

Labs 31-33 move from read-only RACF certificate/key-ring authorization analysis to function-specific RACDCERT delegation and then a controlled synthetic cryptographic-object lifecycle.

This is **RACF-side evidence**. It is not evidence that Policy Agent, AT-TLS or a network TLS path is active.

### FTP-to-JES trust boundary

Lab 34 separates authentication, ingress, JES handoff, JCL processing, execution and SDSF visibility rather than treating them as one authorization decision. Part 2 validates different behavior between TSO/E `SUBMIT` and the FTP/JES level-1 path for the controlled identity.

### TN3270 exposure

Lab 35 validates a limited authentication-response distinction. Lab 36 then moves down the stack and validates cleartext TN3270 transport on the tested path.

### Failure evidence as engineering evidence

Lab 37 Part 1 does not fabricate a successful NC110 run. It validates OMVS, native TCP/IP visibility and compiler readiness, then isolates the LCS/ETH1 connectivity blocker and closes Part 1 at that boundary.

---

## Cross-domain ownership

RACF is cross-cutting, but subsystem ownership remains explicit.

| Domain | RACF repository owns | Target repository owns |
|---|---|---|
| USS | OMVS identity, UID/GID, UNIXMAP, UNIXPRIV, effective authorization | Shell, POSIX processes, paths and filesystem operations |
| JES2 / SDSF | Security interpretation, general-resource and command authority | JES execution, spool and subsystem engineering |
| Communications Server | RACF identities, SAF controls, certificate/key-ring authorization | TCP/IP, FTP, TN3270, Policy Agent, AT-TLS and network behavior |
| Scheduler | Future service-ID and JES authority analysis | Scheduling semantics and workload control |
| CICS | Future RACF/SAF external-security boundary | CICS resources and transaction engineering |
| Db2 | Future RACF-side subsystem boundary | Db2 administration and SQL authorization |
| Core z/OS | Security interpretation of protected resources | IPL, PARMLIB, system configuration, recovery and infrastructure |

See [Ecosystem Integration](docs/ECOSYSTEM-INTEGRATION.md) for the detailed model.

---

## Evidence method

The portfolio-wide method is:

```text
BUILD -> EXECUTE -> OBSERVE -> DIAGNOSE -> CORRECT -> VALIDATE -> DOCUMENT
```

For security-change work, this repository adds:

```text
BASELINE
   -> DENY / establish current behavior
   -> MINIMAL CHANGE
   -> REFRESH where required
   -> ALLOW / functional validation
   -> ROLLBACK
   -> FUNCTIONAL ROLLBACK VALIDATION
   -> DOCUMENT
```

Evidence may include command output, screenshots, manifests, before/after states, functional tests, findings, limitations and rollback records.

---

## Publication security

Before public publication, evidence is reviewed to avoid unnecessary disclosure of:

- passwords, credentials, tokens or secrets;
- private keys;
- private IP addresses and MAC addresses;
- host adapter identifiers;
- terminal/session identifiers;
- host-side infrastructure details unrelated to the objective.

Synthetic identities and narrow changes are preferred wherever possible.

---

## Current boundary and roadmap

### Validated in this repository

The current evidence demonstrates, among other capabilities:

- RACF user/group/profile analysis;
- dataset protection, UACC and PERMIT;
- controlled identities and functional allow/deny testing;
- audit and violation evidence;
- FACILITY and OPERCMDS analysis;
- controlled hardening and rollback;
- OMVS identity, UNIXMAP and UNIXPRIV analysis;
- least-privilege UNIXPRIV delegation;
- RACF certificate/key-ring authorization;
- function-specific RACDCERT delegation;
- RACLIST effective-authority behavior;
- synthetic certificate/key-ring lifecycle;
- FTP-to-JES trust-boundary analysis;
- TN3270 authentication-response analysis;
- TN3270 cleartext transport validation;
- OMVS/NC110 readiness through Lab 37 Part 1.

### Cross-domain / requires evidence

The following are not presented as completed end-to-end capabilities:

- consumption of `LAB33CERT` / `LAB33RING` by a network service;
- Policy Agent / TTLSRule / AT-TLS validation using the retained RACF objects;
- scheduler service-ID least-privilege integration;
- end-to-end SERVAUTH integration;
- CICS external RACF security integration;
- Db2 external RACF integration;
- cross-repository SMF security-event correlation;
- Lab 37 NC110 execution / Part 2.

---

## Engineering principles

1. Establish a baseline before changing security state.
2. Analyze **effective authority**, not profile presence alone.
3. Grant the smallest capability required by the test.
4. Prefer controlled test identities over critical service IDs.
5. Validate functionally; administrative command success is not enough.
6. Plan rollback before change.
7. Validate the rollback behavior.
8. Preserve failures when they establish a real boundary or limitation.
9. Separate laboratory observations from production claims.
10. Keep subsystem ownership explicit.

---

## Environment

Evidence is produced in a controlled personal mainframe laboratory using IBM z/OS ADCD / Hercules, 3270 / TSO/E, ISPF, SDSF, RACF, z/OS UNIX / OMVS and the native RACF/SAF facilities available in the environment.

Observed behavior can be installation- and release-dependent.

---

## Portfolio position

```text
GitHub Profile
      |
      v
z/OS Engineering Portfolio
      |
      v
Security & Compliance
      |
      v
mainframe-racf-security-evidence
      |
      +--> identity & access
      +--> effective authority
      +--> least privilege
      +--> audit evidence
      +--> rollback
      +--> cryptographic trust
      +--> cross-domain trust boundaries
```

**Repository role:** provide evidence-backed RACF/SAF security engineering while leaving subsystem-specific engineering to the repositories that own those technologies.

---

## Academy bridge — Communications Server + RACF/SAF

Network security on z/OS crosses repository boundaries. Communications Server owns the TCP/IP service and policy context; SAF/RACF owns identity, protected-resource authorization and RACF-managed cryptographic objects.

**Existing learning chain:** Labs 31–33 build certificate/key-ring authority and lifecycle → [Communications Lab 21](https://github.com/P-dot/zos-communications-server-network-lab/tree/main/labs/21-policy-agent-attls-readiness-assessment) assesses AT-TLS readiness → [Lab 22](https://github.com/P-dot/zos-communications-server-network-lab/tree/main/labs/22-controlled-policy-agent-attls-implementation) reaches controlled TTLS enablement → [Lab 23](https://github.com/P-dot/zos-communications-server-network-lab/tree/main/labs/23-http-attls-integration-readiness-part-1) consumes the security-side identity/trust context.

This is a cross-domain course path, not duplicated ownership.

[Open the z/OS Engineering Academy →](https://github.com/P-dot/P-dot/blob/main/docs/ACADEMY.md)

---

## Academy bridge — Communications Server + RACF/SAF

Network security on z/OS crosses domain boundaries. Communications Server owns the TCP/IP service and policy context; SAF/RACF owns identity and protected-resource authorization.

**Existing learning chain:** cryptographic-trust Labs 31–33 → [Communications AT-TLS readiness](https://github.com/P-dot/zos-communications-server-network-lab/tree/main/labs/21-policy-agent-attls-readiness-assessment) → [controlled TTLS enablement](https://github.com/P-dot/zos-communications-server-network-lab/tree/main/labs/22-controlled-policy-agent-attls-implementation) → [HTTP AT-TLS integration readiness](https://github.com/P-dot/zos-communications-server-network-lab/tree/main/labs/23-http-attls-integration-readiness-part-1).

[Open the z/OS Engineering Academy →](https://github.com/P-dot/P-dot/blob/main/docs/ACADEMY.md)


---

## z/OS Engineering Academy

**Academy role:** Security School — SAF/RACF identity, authorization and trust boundaries.

[Start the Academy](https://github.com/P-dot/P-dot/blob/main/docs/ACADEMY.md) · [Course Catalog](https://github.com/P-dot/P-dot/blob/main/docs/COURSES.md) · [Curriculum Graph](https://github.com/P-dot/P-dot/blob/main/docs/CURRICULUM.md) · [Cross-Domain Relationships](https://github.com/P-dot/P-dot/blob/main/docs/RELATIONSHIPS.md)

> Learn the concept → execute the lab → interpret the evidence → understand the subsystem boundary → continue to the next connected course.
