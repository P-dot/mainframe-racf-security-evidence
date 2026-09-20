# Lab 35 — TN3270 / TSO Authentication Exposure and User-Enumeration Resistance

## Objective

Validate, with a small controlled identity set, whether the TN3270 / TSO/E logon path in the z/OS V1R11 ADCD/Hercules laboratory exposes distinguishable authentication responses that can reveal whether an identity is TSO-enabled.

The lab is intentionally narrow. It does not perform password spraying, dictionary attacks, credential guessing loops, or uncontrolled user enumeration. It compares three known test states and one single incorrect-password event.

## Context

The Communications Server portfolio had already established that TN3270 is active on classic TCP port 23 and mapped to TSO. This security lab therefore focuses on the RACF/TSO authentication behavior exposed through that service rather than repeating network-service discovery.

## Controlled identities

| Identity | Known state before test |
|---|---|
| `H7USER` | RACF identity with a usable TSO path |
| `USER2` | RACF identity present but not authorized to use TSO |
| `H7NOUSR` | Deliberately nonexistent test userid |

No administrative userid was used for negative authentication tests.

## Evidence-driven test

### 1. RACF identity baseline

`LISTUSER H7USER` confirmed the controlled H7USER identity. `LISTUSER USER2` confirmed USER2 exists in RACF. `LISTUSER H7NOUSR` returned:

```text
ICH30001I UNABLE TO LOCATE USER ENTRY H7NOUSR
```

This independently established the identity states before interpreting the TN3270 responses.

### 2. TN3270 runtime baseline

The runtime query showed TN3270 listening on TCP port 23:

```text
TN3270   0.0.0.0..23   LISTEN
```

This confirms the interactive TSO access path was active during the test.

### 3. Existing RACF userid without usable TSO access

At the TSO/E logon panel, `USER2` produced:

```text
IKJ56420I Userid USER2 not authorized to use TSO
```

### 4. Nonexistent userid

The deliberately nonexistent `H7NOUSR` produced the same response class:

```text
IKJ56420I Userid H7NOUSR not authorized to use TSO
```

The tested panel therefore did not distinguish these two states by message text.

### 5. TSO-enabled userid

`H7USER` progressed to the normal password-entry state. A single deliberately incorrect password produced:

```text
IKJ56421I PASSWORD NOT AUTHORIZED FOR USERID
IKJ56429A REENTER -
```

A later correct authentication for the same controlled userid successfully progressed into the TSO/ISPF environment. The publication evidence for that final success is sanitized to exclude the ADCD sample credential table shown by the installation banner.

## Result

**PASS — a limited authentication-response distinction was validated.**

The tested TN3270 / TSO/E path distinguishes a TSO-enabled userid with an invalid password from identities that either do not have usable TSO access or do not exist:

```text
                    TN3270 / TSO LOGON
                           |
          +----------------+----------------+
          |                                 |
       H7USER                         USER2 / H7NOUSR
  TSO-enabled userid                  no usable TSO path
          |                                 |
    wrong password                          |
          |                                 |
          v                                 v
      IKJ56421I                          IKJ56420I
 PASSWORD NOT AUTHORIZED          not authorized to use TSO
```

The evidence does **not** demonstrate complete RACF userid enumeration: `USER2` and `H7NOUSR` produced the same TSO message in the tested path.

## Security interpretation

A security reviewer should treat the observable distinction as an authentication-surface information leak rather than automatically labeling it a product vulnerability. The practical risk depends on service exposure, identity naming conventions, monitoring, password policy, network controls, and compensating protections.

The important engineering lesson is that a network-facing authentication service should be assessed for response consistency as well as for credential correctness.

## Repository relationship

- Communications Server owns the TN3270 listener, port/configuration and transport-hardening work.
- This repository owns the RACF/TSO authentication interpretation and controlled identity evidence.

## Contents

- `commands/lab35-commands.txt` — controlled test flow.
- `docs/findings.md` — evidence-derived findings.
- `docs/security-analysis.md` — risk interpretation and limitations.
- `docs/video-mapping.md` — mapping from the source-video concept to the defensive lab.
- `docs/next-step.md` — recommended continuation.
- `evidence/README.md` — evidence manifest and publication notes.
- `evidence/screenshots/` — publication-safe screenshots.

## Closure

Lab 35 closes the manual validation of the source video's TSO user-enumeration concept. A large-scale brute-force exercise is not required to establish the underlying behavior.
