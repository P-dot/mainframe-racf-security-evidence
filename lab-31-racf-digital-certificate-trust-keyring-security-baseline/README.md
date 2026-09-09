# Lab 31 — RACF Digital Certificate Trust & Key Ring Security Baseline

## Objective

Establish a read-only RACF security baseline for digital certificate and key-ring administration in the z/OS ADCD/Hercules laboratory, then validate effective authorization with the controlled non-privileged `H7USER` identity.

This lab does **not** create, import, connect, export, alter or delete certificates or private keys.

## Why this lab exists

A previous Communications Server lab already inventoried certificate material and service-oriented key-ring usage from a network-security perspective. This RACF lab has a different responsibility:

> determine how RACF protects certificate and key-ring administration, which `IRR.DIGTCERT.*` controls are present, and whether a non-privileged identity can effectively use `RACDCERT`.

This avoids duplicating subsystem-specific certificate inventory while extending the RACF/SAF security track into cryptographic authorization.

## Engineering method

```text
Inventory
   |
   v
RACF FACILITY baseline
   |
   v
Identify defined IRR.DIGTCERT.* controls
   |
   v
Analyze ACL / UACC
   |
   v
Controlled H7USER negative tests
   |
   v
Confirm no state change
   |
   v
Document next delegation experiment
```

## Commands executed

### Administrative baseline — IBMUSER

```text
RLIST FACILITY IRR.DIGTCERT.LISTRING ALL
RLIST FACILITY IRR.DIGTCERT.GENCERT ALL
RLIST FACILITY IRR.DIGTCERT.ADD ALL
RLIST FACILITY IRR.DIGTCERT.ADDRING ALL
RLIST FACILITY IRR.DIGTCERT.CONNECT ALL
RACDCERT SITE LIST
SEARCH CLASS(FACILITY) MASK(IRR.DIGTCERT)
RLIST FACILITY IRR.DIGTCERT.LIST ALL
```

### Effective-authority validation — H7USER

```text
RLIST FACILITY IRR.DIGTCERT.LISTRING ALL
RACDCERT ID(H7USER) LISTRING(*)
RACDCERT ID(IBMUSER) LISTRING(*)
RACDCERT ID(H7USER) ADDRING(LAB31RING)
```

## Observed RACF control state

Two `IRR.DIGTCERT.*` FACILITY profiles were observed as defined:

```text
IRR.DIGTCERT.LIST
IRR.DIGTCERT.LISTRING
```

Both showed the same effective pattern in the captured evidence:

```text
OWNER        IBMUSER
UACC         NONE
IBMUSER      ALTER
WSCFG1       READ
XSCFG        READ
AUDIT        FAILURES(READ)
WARNING      NO
```

The following queried resources were not found:

```text
IRR.DIGTCERT.GENCERT
IRR.DIGTCERT.ADD
IRR.DIGTCERT.ADDRING
IRR.DIGTCERT.CONNECT
```

`RACDCERT SITE LIST` returned no SITE certificate information in the observed environment.

## Effective-authority validation

`H7USER` was used as the controlled non-privileged subject.

The following operations were denied:

```text
RLIST FACILITY IRR.DIGTCERT.LISTRING ALL
RACDCERT ID(H7USER) LISTRING(*)
RACDCERT ID(IBMUSER) LISTRING(*)
RACDCERT ID(H7USER) ADDRING(LAB31RING)
```

The key RACDCERT result was:

```text
IRRD101I You are not authorized to issue the RACDCERT command.
```

This is significant because the absence of a specifically named administrative profile such as `IRR.DIGTCERT.ADDRING` did **not** result in uncontrolled key-ring creation by the non-privileged test identity.

## Security interpretation

The lab demonstrates the distinction between:

```text
profile absence
      !=
automatic effective permission
```

and reinforces the repository-wide principle:

```text
configuration inspection
      +
functional validation
      =
stronger security evidence
```

The environment showed explicit protection for RACDCERT list/list-ring functionality and a functional authorization barrier against the controlled test identity.

No vulnerability claim is made from the absence of the other queried `IRR.DIGTCERT.*` profiles. Effective authority depends on the complete RACF command authorization model and installation context.

## Change and rollback status

No RACF object was successfully created or modified.

```text
LAB31RING created   NO
Certificate change  NO
Private-key change  NO
RACF profile change NO
Rollback required   NO
```

The lab therefore closes as a baseline and negative effective-authority validation exercise.

## Evidence

Evidence is stored under `evidence/`.

The public evidence set should be reviewed before publication for certificate labels, distinguished names, serial numbers, certificate IDs, key-ring names, hostnames, email addresses and other environment-specific identifiers.

Private keys must never be published.

## Relationship to the wider z/OS lab ecosystem

```text
Communications Server Lab 09
certificate / network-service inventory
             |
             v
RACF Lab 31
cryptographic authorization baseline
             |
             v
RACF Lab 32
controlled cryptographic delegation
             |
             v
future RACF <-> System SSL / Communications integration
```

The Communications Server repository owns network-service and TLS engineering. This repository owns RACF/SAF authorization, effective authority, least privilege and controlled security validation.

## Result

Lab 31 establishes the first dedicated RACF cryptographic-security baseline in the main RACF/SAF security series.

The lab confirms that:

- `IRR.DIGTCERT.LIST` and `IRR.DIGTCERT.LISTRING` are explicitly protected;
- both observed profiles use `UACC(NONE)`;
- the controlled non-privileged identity cannot inspect the protected FACILITY definition;
- the controlled identity cannot issue the tested RACDCERT operations;
- an attempted test key-ring creation was denied;
- no system state required rollback.

The next lab moves from read-only/negative validation to a deliberately controlled least-privilege delegation lifecycle.

---

# Planned Lab 32 — RACF Controlled Cryptographic Delegation & Key Ring Validation

## Planned objective

Demonstrate a controlled RACF certificate/key-ring authorization lifecycle using `H7USER`, with the smallest authority required for one carefully selected RACDCERT operation.

The intended pattern is:

```text
DENIED
   |
   v
minimal RACF authorization
   |
   v
ALLOWED
   |
   v
verify resulting key-ring state
   |
   v
remove authorization
   |
   v
rollback created test object if applicable
   |
   v
DENIED again
```

## Planned scope

Lab 32 will be designed only after validating the exact z/OS V1R11 RACDCERT authority requirements for the chosen operation.

Candidate scope:

- define or use the minimum required `IRR.DIGTCERT.*` FACILITY control;
- grant only the minimum access level to `H7USER`;
- refresh RACF only where required;
- perform one narrowly scoped RACDCERT operation using a laboratory-only object;
- verify effective access;
- remove the temporary permission;
- remove any laboratory key ring created by the test;
- validate return to the original denied state;
- preserve before/after and rollback evidence.

## Explicit exclusions for Lab 32

Unless the validated procedure specifically requires them, Lab 32 will not:

- grant `SPECIAL`;
- grant `OPERATIONS`;
- grant UID(0);
- expose a real service key ring;
- export a private key;
- publish private-key material;
- modify production-like TCP/IP service identities;
- alter Communications Server configuration.

The goal is **least-privilege cryptographic administration**, not broad certificate administration.
