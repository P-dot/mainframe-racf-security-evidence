# Lab 34 — Part 2: Channel-Dependent FTP-to-JES Authorization Validation

## Objective

Extend the Part 1 FTP-to-JES baseline by replacing the original malformed job with a syntactically valid, harmless `IEFBR14` workload and then comparing two submission channels used by the same controlled RACF identity: TSO/E `SUBMIT` and the z/OS FTP JES interface.

The purpose is not to claim a product vulnerability. The purpose is to prove, with evidence, that command authority, network authentication, JES ingress, execution identity, and batch execution are distinct security boundaries in the tested z/OS V1R11 ADCD/Hercules environment.

## Starting point

Part 1 had already established:

```text
H7USER -> FTP authentication -> SITE FILE=JES -> JES internal reader -> JES job ID
```

but the submitted JCL produced a conversion error. Part 2 removes that ambiguity first.

## Controlled workload

A new two-line member was created to avoid continuation and formatting ambiguity:

```jcl
//H7FTP2 JOB (LAB),'FTP JES P2',CLASS=A,MSGCLASS=X,NOTIFY=H7USER
//STEP1 EXEC PGM=IEFBR14
```

The program is intentionally harmless and does not allocate, alter, or delete protected resources.

## Test sequence

### 1. TSO/E submission path

The controlled user attempted:

```text
SUBMIT 'H7USER.JCL.LAB(FTPJES2)'
```

Observed result:

```text
IKJ56251I USER NOT AUTHORIZED FOR SUBMIT
IKJ56251I YOUR TSO ADMINISTRATOR MUST AUTHORIZE USE OF THIS COMMAND
```

Inspection of `TSOAUTH JCL` showed `UACC(NONE)`, and `H7USER` was not present in the access list.

### 2. FTP/JES submission path

Using the same RACF identity, the FTP server accepted `SITE FILE=JES` and the same controlled JCL member was transferred to the JES internal reader.

Observed result:

```text
125 Sending Job to JES internal reader FIXrecfm 80
250-It is known to JES as JOB07465
250 Transfer completed successfully.
```

### 3. Execution proof

The resulting JES spool demonstrated that the job was associated with `H7USER` and completed normally:

```text
IRR010I USERID H7USER IS ASSIGNED TO THIS JOB.
$HASP373 H7FTP2 STARTED
$HASP395 H7FTP2 ENDED
```

`JESJCL` showed the expected `IEFBR14` stream, and `JESYSMSG` recorded:

```text
IEF142I H7FTP2 STEP1 - STEP WAS EXECUTED - COND CODE 0000
```

Therefore Part 2 proves successful batch execution through the FTP/JES path, not merely handoff to the internal reader.

## Authorization observations

### TSOAUTH

`TSOAUTH JCL` is a real negative boundary in this environment: `H7USER` cannot use the TSO/E `SUBMIT` command.

### H7USER identity

`LISTUSER H7USER` showed `ATTRIBUTES=NONE`. The observed FTP/JES execution was therefore not explained by `SPECIAL`, `OPERATIONS`, or another broad RACF administrative attribute.

### JESJOBS

The class is active in the tested RACF configuration, but `SEARCH CLASS(JESJOBS)` returned no matching profiles.

This is recorded as an observed configuration fact only; the lab does not infer an authorization decision that was not explicitly evidenced.

### JESINPUT

The class is active, but `SEARCH CLASS(JESINPUT)` returned no matching profiles.

The successful FTP/JES run therefore leaves an open implementation/configuration question concerning the exact effective SAF path used by this FTP/JES level-1 submission in the tested installation. That question is deliberately not overstated as a bypass or vulnerability.

### SURROGAT

`SEARCH CLASS(SURROGAT)` identified `IBMUSER.SUBMIT`, but `RLIST SURROGAT IBMUSER.SUBMIT ALL` showed `UACC(NONE)` and no `H7USER` access entry.

The spool independently showed that the job ran as `H7USER`, so surrogate execution under `IBMUSER` does not explain the result.

## Internal-reader state

A later `$D RDI(*)` query returned no selectable reader entries. That is retained as a point-in-time observation after the submitted job had already completed and is not treated as proof that no dynamic internal reader had existed during the FTP submission.

`$D INTRDR` then established the global JES2 internal-reader characteristics:

```text
AUTH=(DEVICE=NO,JOB=YES,SYSTEM=YES)
BATCH=YES
CLASS=A
HOLD=NO
SYSAFF=(ANY)
TRACE=NO
```

The security-relevant fact for this lab is that the internal-reader configuration permits batch processing (`BATCH=YES`) and defaults to class `A`, matching the harmless test job.

## Result

**PASS — channel-dependent authorization behavior validated.**

The same controlled RACF identity produced two different outcomes:

```text
                         H7USER
                            |
              +-------------+-------------+
              |                           |
              v                           v
         TSO/E SUBMIT                  FTP/JES
              |                           |
        TSOAUTH JCL                        v
         UACC(NONE)                 internal reader
              |                       BATCH=YES
           DENIED                         |
                                          v
                                       JOB07465
                                          |
                                 identity = H7USER
                                          |
                                          v
                                      IEFBR14
                                          |
                                          v
                                       CC 0000
```

The defensible conclusion is:

> TSO/E `SUBMIT` command authority and FTP/JES workload-submission capability are distinct authorization paths in the tested environment.

The lab does **not** claim a RACF bypass, a JES2 vulnerability, or arbitrary privileged code execution.

## Closure

Part 2 closes the original uncertainty from Part 1: valid JCL can traverse the FTP/JES path and execute successfully as the authenticated RACF user.

A deeper investigation of the exact `JESINPUT` / `JESJOBS` SAF decision path is useful as a future hardening exercise, but it is not required before continuing with the next threat pattern from the source video.
