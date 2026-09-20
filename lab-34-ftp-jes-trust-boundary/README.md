# Lab 34 — FTP-to-JES Trust Boundary

## Controlled ingress validation and security-boundary analysis

This lab examines a security-relevant cross-domain path in a controlled z/OS V1R11 ADCD/Hercules environment: an authenticated RACF identity entering the system through the z/OS FTP server and using the FTP JES interface to hand JCL to the JES internal reader.

The experiment was motivated by a training-video attack path built around FTP `SITE FILE=JES`. The video is treated only as a threat hypothesis. The repository records what the local system actually demonstrated, separates each trust transition, and avoids treating a successful transport step as proof of downstream authority or execution.

## Security question

> How far can a controlled, non-administrative RACF identity progress from an exposed FTP service toward JES workload submission, and which observations belong to FTP, JES, JCL processing, RACF/SAF, and SDSF respectively?

The evidence establishes that `H7USER` authenticated to FTP, entered JES mode, transferred a controlled JCL stream, and received a JES job identifier. That proves an operational **FTP-to-JES ingress path** in the tested environment.

It does **not** by itself prove that arbitrary workload execution is authorized. The captured evidence also contains an SDSF authorization denial and a later JCL-processing failure; those are separate controls/events and must not be conflated with the FTP handoff.

## Environment

- z/OS V1R11 ADCD on Hercules
- TCP/IP started task: `TCPIP`
- FTP server address space: `FTPD1`
- FTP service: TCP/21
- Controlled RACF test identity: `H7USER`
- Controlled JCL member: `H7USER.JCL.LAB(FTPJES1)`
- Intended harmless program: `IEFBR14`

## Method

```text
UNDERSTAND
   -> DISCOVER
   -> MAP TRUST
   -> CONTROLLED TEST
   -> OBSERVE
   -> CLASSIFY EVIDENCE
   -> DOCUMENT
   -> PLAN NEXT CONTROL TEST
```

No broad privilege is introduced in Part 1. The purpose is to establish the path and classify the evidence correctly before any authorization change is considered.

## Validated path

```text
Network exposure
      |
      v
TCP/21 -> FTPD1 LISTEN
      |
      v
RACF authentication: H7USER                 OBSERVED
      |
      v
FTP runtime: JESINTERFACELEVEL = 1          OBSERVED
      |
      v
SITE FILE=JES                               OBSERVED
      |
      v
Controlled JCL transfer
      |
      v
FTP -> JES internal reader                  OBSERVED
      |
      v
JES job identifier assigned                 OBSERVED
      |
      +----------------------------+
      |                            |
      v                            v
JCL processing                SDSF access
observed failure              observed denial
      |                            |
      v                            v
execution not proven          separate UI/security boundary
```

## Key observations

### 1. Runtime exposure was proven separately from static configuration

`NETSTAT PORTLIST` identified configured FTP-related port ownership, while `NETSTAT CONN` showed `FTPD1` actually listening on TCP/21. Static configuration and runtime exposure are therefore documented as separate facts.

### 2. FTPD1 was correlated with the TCP/IP configuration

`/D A,FTPD1` and `NETSTAT,ALL,CLIENT=FTPD1` correlated the running FTP address space with the listener. `ADCD.Z111S.TCPPARMS(PROF1)` then connected runtime state to `AUTOLOG` / `PORT` configuration.

### 3. Effective FTP JES capability was observed at runtime

The authenticated FTP `STAT` output reported values including:

```text
JESLRECL is 80
JESRECFM is Fixed
JESINTERFACELEVEL is 1
```

This is stronger evidence of the active server behavior than relying only on commented or default values in `FTP.DATA`.

### 4. H7USER authenticated successfully to FTP

The controlled RACF identity entered the network service successfully. This proves the identity was accepted at the FTP authentication boundary; it does not imply authority at later JES, dataset, command, or SDSF boundaries.

### 5. The FTP JES interface was reachable

`SITE FILE=JES` was accepted and JES-oriented interaction became available within the FTP session.

### 6. The internal-reader handoff occurred

The FTP transfer returned an acknowledgement that the data stream was sent to the JES internal reader and assigned a JES job identifier (`JOB07414` in the captured run).

A job identifier is evidence that the submission stream reached JES. It is **not** equivalent to successful execution.

### 7. The captured downstream evidence must be classified correctly

The evidence includes an SDSF message of the form:

```text
ISF024I USER H7USER NOT AUTHORIZED TO SDSF, NO GROUP ASSIGNMENT
```

That message is an **SDSF authorization** result. It must not be presented as proof that JES rejected the FTP submission itself.

The job evidence also showed a JCL-processing failure, including `JCL ERROR 312` / `IEF650I UNIDENTIFIED OPERATION FIELD` in the captured run. That is a **JCL syntax/processing** outcome, not a RACF authorization denial.

These distinctions are central to the lab.

## Evidence classification matrix

| Observation | Domain | What it proves | What it does not prove |
|---|---|---|---|
| TCP/21 LISTEN under FTPD1 | Communications Server | FTP service is active | External reachability from every network |
| H7USER FTP login | RACF + FTP | Identity authenticated to FTP | JES execution authority |
| `JESINTERFACELEVEL=1` | FTP runtime | JES interface enabled in tested server path | Workload authorization |
| `SITE FILE=JES` accepted | FTP application layer | Session entered JES mode | Job execution |
| `125 Sending Job to JES internal reader` | FTP/JES boundary | Stream handed toward JES | Successful execution |
| JES job identifier returned | JES ingress | JES recognized the submitted stream | Valid JCL or authorized resource use |
| `ISF024I ... NOT AUTHORIZED TO SDSF` | SDSF | H7USER lacked the tested SDSF access/group mapping | FTP submission denial |
| `JCL ERROR 312` / `IEF650I` | JCL/JES conversion | Submitted JCL contained/produced a processing error | RACF denial |

## Why this belongs in the security repository

The network repository owns FTP service engineering and transport behavior. This lab is different: its main subject is **trust propagation and effective authority across subsystem boundaries**.

The security value is the separation of:

```text
service exposure
   !=
authentication
   !=
application capability
   !=
JES ingress
   !=
valid JCL
   !=
resource authorization
   !=
SDSF authorization
   !=
workload execution
```

That directly extends the repository's existing work on controlled identities, JES2/SDSF, effective authority, least privilege, and evidence-driven validation.

## Result

**PARTIAL / BASELINE ESTABLISHED** — the controlled FTP-to-JES ingress path was validated, but the captured run does not yet isolate a RACF/JES authorization denial at the execution boundary. SDSF denial and JCL failure are preserved as separate evidence.

The next phase must first obtain a syntactically valid harmless job and then test downstream authorization deliberately, without broad privilege and with rollback.

## Repository contents

- `commands/lab34-part1-commands.txt` — executed flow and evidence interpretation.
- `docs/architecture.md` — trust-boundary model.
- `docs/findings.md` — evidence-derived findings.
- `docs/security-analysis.md` — security interpretation and limitations.
- `docs/video-threat-model.md` — what the source video contributes and what is intentionally not copied.
- `docs/next-part.md` — precise continuation plan.
- `samples/FTPJES1.jcl` — intended harmless workload sample; validate formatting before reuse.
- `evidence/screenshots/README.md` — publication-safe evidence checklist.

## Publication safety

Before publication, screenshots must be checked for credentials, non-loopback IP addresses, MAC addresses, adapter identifiers, terminal/session identifiers, local host paths, and unrelated infrastructure details. Redaction must preserve the technical evidence needed for the conclusion.
