# Lab 32 Plan — RACF Controlled Cryptographic Delegation & Key Ring Validation

## Purpose

Move from Lab 31's baseline and denied effective-access proof into a controlled least-privilege delegation lifecycle.

## Planned engineering sequence

```text
Confirm baseline denial
        |
        v
Define/select exact RACF control
        |
        v
Grant minimum documented access
        |
        v
Refresh only if required
        |
        v
Perform one controlled RACDCERT action
        |
        v
Verify object/effective access
        |
        v
Remove temporary authorization
        |
        v
Remove test object if created
        |
        v
Validate denial again
```

## Candidate test

A likely candidate is creation and validation of a laboratory-only key ring for `H7USER`, but the exact command and required FACILITY access level must first be validated against the z/OS V1R11 RACF documentation.

## Success criteria

Lab 32 should not be considered complete unless it demonstrates all of the following:

1. initial denial;
2. minimal RACF change;
3. functional allow result;
4. controlled test-object verification;
5. permission removal;
6. object cleanup where applicable;
7. final denial;
8. evidence of restored security state.

## Safety constraints

Do not use a production-like service identity. Do not export private keys. Do not publish certificate secrets. Do not grant broad RACF privilege merely to make RACDCERT work.
