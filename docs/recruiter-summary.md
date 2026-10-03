# Recruiter Summary — RACF / z/OS Security Engineering Evidence

## Repository profile

`mainframe-racf-security-evidence` is a hands-on IBM z/OS security-engineering portfolio built in a controlled ADCD / Hercules environment.

The repository has progressed well beyond introductory RACF inquiry commands. Its evidence covers **identity and access management, effective authorization, least privilege, controlled hardening, functional validation, rollback, RACF/SAF cross-domain controls, cryptographic authorization and trust-boundary analysis**.

## Demonstrated capability

The published labs demonstrate practical work with:

- RACF users, groups and privileged attributes;
- DATASET profiles, UACC, PERMIT and group-based access;
- controlled test identities and functional allow/deny testing;
- violation and audit evidence;
- started-task and technical-user security review;
- OMVS identity, UID/GID, UNIXMAP and UNIXPRIV;
- FACILITY and OPERCMDS;
- effective-authority analysis rather than profile inspection alone;
- controlled least-privilege delegation;
- RACLIST refresh and runtime-authority validation;
- rollback plus functional rollback proof;
- RACF digital certificates and key rings;
- `IRR.DIGTCERT.*` / RACDCERT authorization;
- controlled certificate/key-ring lifecycle;
- FTP-to-JES trust-boundary analysis;
- TN3270/TSO authentication-response analysis;
- TN3270 cleartext-transport validation;
- evidence-driven blocker isolation at the OMVS/network boundary.

## Engineering approach

The strongest labs use a repeatable security-control lifecycle:

```text
BASELINE
   -> establish effective behavior
   -> make the minimum controlled change
   -> refresh runtime state where required
   -> validate functionally
   -> rollback
   -> validate rollback
   -> preserve evidence
```

This distinguishes administrative configuration from actual effective authority.

## Selected evidence milestones

### Least privilege

The UNIXPRIV work demonstrates a controlled:

```text
DENIED -> minimum grant -> ALLOWED -> rollback -> DENIED
```

without solving the test by granting broad global privilege.

### Cryptographic authorization

Labs 31-33 progress from RACF certificate/key-ring authorization analysis to function-specific RACDCERT delegation and a synthetic certificate/key-ring lifecycle.

The repository does not misrepresent the retained RACF cryptographic objects as proof of an active network TLS implementation.

### Cross-domain security

Later labs examine RACF/SAF security at real subsystem boundaries while keeping ownership explicit:

- FTP/JES ingress and channel-dependent authorization;
- TN3270/TSO authentication behavior;
- cleartext TN3270 transport;
- OMVS/network readiness and blocker isolation.

## Evidence discipline

The repository preserves:

- baseline and final state;
- positive and negative tests;
- command output and screenshots where appropriate;
- limitations and failed paths;
- rollback procedures and rollback proof;
- publication-security review.

Failed execution is retained when it establishes a genuine platform or connectivity boundary rather than being rewritten as success.

## Professional relevance

The evidence is directly relevant to work involving:

- IBM z/OS Security;
- RACF administration and support;
- IAM / privileged-access review;
- mainframe operations and production support;
- security operations / SecOps;
- infrastructure security;
- IT audit and control validation;
- least-privilege and hardening activities.

## Scope boundary

This is laboratory evidence, not a production security certification.

RACF/SAF security interpretation belongs to this repository; subsystem engineering remains with the corresponding portfolio domains such as Communications Server, USS, JES2/core z/OS, CICS, Db2 and workload automation.

## Navigation

- [Repository README](../README.md)
- [Ecosystem Integration](ECOSYSTEM-INTEGRATION.md)
- [Main Portfolio](https://github.com/P-dot/P-dot)
- [Architecture V2](https://github.com/P-dot/zos-adcd-hercules-engineering-lab/tree/main/docs/architecture/v2)
